FROM ubuntu:22.04

# The URL of Robot Shop
ENV HOST="http://web:8080"

# Install Chromium and Chrome WebDriver
RUN \
    apt-get update && \
    apt-get install -y wget curl unzip gnupg2 python3 python3-pip && \
    ln -s /usr/bin/python3 /usr/bin/python && \
    \
    python -m pip install --upgrade --no-cache-dir selenium selenium-wire 'blinker==1.7.0' 'pyOpenSSL<24.0.0' 'cryptography<42.0.0' && \
    \
    wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google-chrome.list && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y google-chrome-stable && \
    \
    CHROME_VERSION=$(google-chrome --version | awk '{print $3}' | cut -d '.' -f 1) && \
    echo "Chrome version: $CHROME_VERSION" && \
    CHROMEDRIVER_VERSION=$(curl -sS "https://googlechromelabs.github.io/chrome-for-testing/LATEST_RELEASE_${CHROME_VERSION}") && \
    echo "ChromeDriver version: $CHROMEDRIVER_VERSION" && \
    mkdir -p /opt/chromedriver-$CHROMEDRIVER_VERSION && \
    curl -sS -o /tmp/chromedriver.zip "https://storage.googleapis.com/chrome-for-testing-public/${CHROMEDRIVER_VERSION}/linux64/chromedriver-linux64.zip" && \
    unzip -qq /tmp/chromedriver.zip -d /tmp && \
    mv /tmp/chromedriver-linux64/chromedriver /opt/chromedriver-$CHROMEDRIVER_VERSION/ && \
    chmod +x /opt/chromedriver-$CHROMEDRIVER_VERSION/chromedriver && \
    ln -fs /opt/chromedriver-$CHROMEDRIVER_VERSION/chromedriver /usr/local/bin/chromedriver && \
    \
    rm -rf /tmp/chromedriver.zip /tmp/chromedriver-linux64

RUN useradd -m loader
USER loader

WORKDIR /load

COPY --chown=loader:loader entrypoint.sh .
COPY --chown=loader:loader load.py .

ENTRYPOINT ["./entrypoint.sh"]
