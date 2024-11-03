# PostgreSQL-database-.NET-ASP.NET-REST-API
PostgreSQL database &amp; .NET/ASP.NET REST API on Ubuntu 24 Server 

POSTGRESS DB  

# PostgreSQL Deployment on Ubuntu 24

This guide outlines the steps to deploy a PostgreSQL database on an Ubuntu 24 server.

1. Update System Packages

```bash
sudo apt update
```

2. Install PostgreSQL

```bash
sudo apt install postgresql -y
```

3. Verify PostgreSQL Service (Optional)

```bash
sudo systemctl status postgresql
```

4. Access PostgreSQL Shell and Configure Database

```bash
sudo -u postgres psql
```

Execute the following SQL commands:

```sql
-- Create the database
CREATE DATABASE recipe;

-- Create the user and set the password
CREATE USER admin WITH ENCRYPTED PASSWORD '12345';

-- Change the database owner to admin
ALTER DATABASE recipe OWNER TO admin;

-- Grant all privileges to the user for the database
GRANT ALL PRIVILEGES ON DATABASE recipe TO admin;

-- List databases (optional)
\l

-- Change password of an existing user (if applicable)
ALTER USER postgres WITH PASSWORD '123456';

-- Exit PostgreSQL shell
\q

```




DOTNET MD
# Deploy a .NET/ASP.NET REST API on Ubuntu 24

This guide outlines the steps to deploy a .NET/ASP.NET REST API on an Ubuntu 24 server.

1. Update System Packages

```bash
sudo apt update
```

2. Install .NET 7 SDK

```bash
sudo add-apt-repository ppa:dotnet/backports -y
sudo apt install -y dotnet-sdk-7.0
```







3. Install Entity Framework Core Tools

```bash
dotnet tool install --global dotnet-ef --version 7.0.0
export PATH="$PATH:~/.dotnet/tools"
```

4. Clone the Project

```bash
git clone https://github.com/GerromeSieger/RecipeApp-Dotnet.git
cd RecipeApp-Dotnet
```

5. Restore Dependencies

```bash
dotnet restore
```

6. Configure Database

Ensure your database connection string is properly set in appsettings.json

7. Run Migrations

```bash
dotnet ef migrations add InitialMigration
dotnet ef database update
```

8. Run Application

```bash
dotnet run --urls http://0.0.0.0:5000
```

9. Verify Deployment

Open a web browser and navigate to http://publicip:5000/swagger to access the swagger documentation.

## Alternative Deployment Strategy (Using Systemd)

10. Set Up as a Systemd Service

Create a service file:

```bash
sudo nano /etc/systemd/system/dotnet-api.service
```

Add the following content (adjust paths as necessary):

```ini
[Unit]
Description=.NET Web API
After=network.target

[Service]
User=root
Group=www-data
WorkingDirectory=/root/RecipeApp-Dotnet
ExecStart=dotnet run --urls http://0.0.0.0:5000

[Install]
WantedBy=multi-user.target

```

11. Reload Daemon, Start and Enable dotnet-api Service

```bash
sudo systemctl daemon-reload
sudo systemctl start dotnet-api
sudo systemctl enable dotnet-api
sudo systemctl status dotnet-api
```
