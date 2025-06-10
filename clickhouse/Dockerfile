FROM registry.access.redhat.com/ubi8/ubi:latest

# 1. Install yum-utils so we can add external repos
# 2. Add the official ClickHouse .repo
# 3. Import the repo’s GPG key from its repodata
# 4. Install ClickHouse server & client
# 5. Clean up caches
RUN yum install -y yum-utils \
	&& yum-config-manager --add-repo https://packages.clickhouse.com/rpm/clickhouse.repo  \
	&& rpm --import https://packages.clickhouse.com/rpm/stable/repodata/repomd.xml.key \
	&& yum install -y clickhouse-server clickhouse-client \
	&& yum clean all

# Make data & log dirs world-writable for OpenShift’s arbitrary UID policy
RUN mkdir -p /var/lib/clickhouse /var/log/clickhouse-server \
	&& chmod -R 0777 /var/lib/clickhouse /var/log/clickhouse-server

# Fix permissions for OpenShift’s arbitrary UID:
RUN chmod -R 0755 /etc/clickhouse-server \
	&& find /etc/clickhouse-server -type f -name '*.xml' -exec chmod 0644 {} +

# Make data & log dirs writable
RUN mkdir -p /var/lib/clickhouse /var/log/clickhouse-server \
	&& chmod -R 0777 /var/lib/clickhouse /var/log/clickhouse-server

EXPOSE 8123 9000

ENTRYPOINT ["clickhouse-server","--config-file=/etc/clickhouse-server/config.xml"]

