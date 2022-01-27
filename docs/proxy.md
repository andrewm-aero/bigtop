# Building behind an HTTP(s) Proxy

Bigtop leverages a large number of build tools and scripts in order to build the various components, and if you are building behind a proxy, you will need to configure each one.

## Environment Variables (curl, etc)

```bash
# In same shell as build
export HTTP_PROXY=...
export HTTPS_PROXY=...,...
export NO_PROXY=...
export http_proxy=...
export https_proxy=...
export no_proxy=...,...
```

## Gradle

```ini
# $HOME/.gradle/gradle.properties
systemProp.http.proxyHost=...
systemProp.http.proxyPort=...
systemProp.http.proxyUser=...
systemProp.http.proxyPassword=...
systemProp.http.nonProxyHosts=...|...
systemProp.https.proxyHost=...
systemProp.https.proxyPort=...
systemProp.https.proxyUser=...
systemProp.https.proxyPassword=...
systemProp.https.nonProxyHosts=...|...
# If you need to tunnel an https url through a non-https http proxy, uncomment this
# Intentionally left blank
# systemPropjdk.http.auth.tunneling.disabledSchemes=
```

## Maven

```xml
<!-- $HOME/.m2/settings.xml -->

<settings>
  <proxies>
    <!-- this first one is only necessary if you need to tunnel an https url through a non-https http proxy -->
    <proxy>
      <id>http</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>...</username>
      <password>...</password>
      <host>...</host>
      <port>...</port>
      <nonProxyHosts>...|...>/nonProxyHosts>
    </proxy>
    <proxy>
      <id>https</id>
      <active>true</active>
      <username>...</username>
      <password>...</password>
      <host>...</host>
      <port>...</port>
      <nonProxyHosts>...|...>/nonProxyHosts>
    </proxy>
  </proxies>
</settings>
```

```bash
# In the same shell as build
# Only necessary if you if you need to tunnel an https url through a non-https http proxy
# Property intentionally left blank
export MAVEN_OPTS="-Dhttp.auth.tunneling.disabledSchemes= ..."
```

## Ant

```bash
# In the same shell as build
# Make sure to single-quote things, as they will be interpreted as bash shell arguments
export ANT_OPTS="-Dhttp.proxyHost='...' -Dhttp.proxyPort='...' -Dhttp.proxyUser='...' -Dhttp.proxyPassword='...' -Dhttp.nonProxyHosts='...|...' -Dhttps.proxyHost='...' -Dhttps.proxyPort='...' -Dhttps.proxyUser='...' -Dhttps.proxyPassword='...' -Dhttps.nonProxyHosts='...|...'"
# Only necessary if you if you need to tunnel an https url through a non-https http proxy
# Property intentionally left blank
export ANT_OPTS="-Dhttp.auth.tunneling.disabledSchemes= ${ANT_OPTS}"
```

## NPM

```
# $HOME/.npmrc
proxy=...
https-proxy=...
noproxy=...,...
```

## Bower

```json
"$HOME/.bowerrc",
{
  "proxy": "...",
  "https-proxy": "...",
  "no-proxy": "..."
}
```
