# SearXNG-Linux-Build
SearXNG is a free internet metasearch engine which aggregates results from more than 70 search services. Users are neither tracked nor profiled. Additionally, SearXNG can be used over Tor for online anonymity.

```
sudo -H apt-get install -y \
    python3-dev python3-babel python3-venv \
    uwsgi uwsgi-plugin-python3 \
    git build-essential libxslt-dev zlib1g-dev libffi-dev libssl-dev \
    shellcheck
   
sudo -H useradd --shell /bin/bash --system \
    --home-dir "/usr/local/searx" \
    --comment 'Privacy-respecting metasearch engine' searx
sudo -H mkdir "/usr/local/searx"
sudo -H chown -R "searx:searx" "/usr/local/searx"

sudo -H -u searx -i
git clone "https://github.com/searxng/searxng" "/usr/local/searx/searx-src"
python3 -m venv "/usr/local/searx/searx-pyenv"
echo ". /usr/local/searx/searx-pyenv/bin/activate" >>  "/usr/local/searx/.profile"

sudo -H -u searx -i
pip install -U pip
pip install -U setuptools
pip install -U wheel
pip install -U pyyaml

cd "/usr/local/searx/searx-src"
pip install -e .

sudo -H mkdir -p "/etc/searxng"
sudo -H cp "/usr/local/searx/searx-src/utils/templates/etc/searxng/settings.yml" \
             "/etc/searxng/settings.yml"

sudo -H sed -i -e "s/ultrasecretkey/$(openssl rand -hex 16)/g" "/etc/searxng/settings.yml"

sudo -H -u searx -i
cd /usr/local/searx/searx-src
export SEARXNG_SETTINGS_PATH="/etc/searxng/settings.yml"
python searx/webapp.py
```
