FROM mcr.microsoft.com/vscode/devcontainers/python:3.9

# Set environment variables to prevent Python from writing .pyc files
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Install locales package and generate nl_NL.UTF-8 locale
RUN apt-get update && \
    apt-get install -y locales && \
    sed -i '/nl_NL.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen nl_NL.UTF-8 && \
    update-locale LANG=nl_NL.UTF-8

# Set the locale environment variables
ENV LANG nl_NL.UTF-8
ENV LANGUAGE nl_NL:en
ENV LC_ALL nl_NL.UTF-8

# Install system dependencies
RUN apt-get update && apt-get install -y \
    wget \
    gnupg \
    unzip \
    xvfb \
    libxi6 \
    libgconf-2-4 \
    default-jdk \
    && rm -rf /var/lib/apt/lists/*

# Add Google Chrome repository
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    sh -c 'echo "deb [arch=amd64] https://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'

# Install Google Chrome
RUN apt-get update && apt-get install -y google-chrome-stable

# Install ChromeDriver
RUN CHROMEDRIVER_VERSION=$(curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE) \
    && wget -q "https://chromedriver.storage.googleapis.com/${CHROMEDRIVER_VERSION}/chromedriver_linux64.zip" \
    && unzip chromedriver_linux64.zip \
    && mv chromedriver /usr/local/bin/ \
    && rm chromedriver_linux64.zip

# Install necessary Python packages
COPY requirements.txt /tmp/pip-tmp/
RUN pip install --upgrade pip \
    && pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp

# Install SpaCy and download the Dutch language model
RUN pip install spacy && \
    python -m spacy download nl_core_news_lg

# Install Jupyter
RUN pip install jupyter

# Install Selenium
RUN pip install --upgrade webdriver_manager

RUN python -m nltk.downloader -d /usr/local/share/nltk_data all

# install stanza
RUN pip install stanza

# download the Dutch model
RUN python -c "import stanza; stanza.download('nl')"
