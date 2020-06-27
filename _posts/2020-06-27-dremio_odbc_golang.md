---
layout: post
title: "Querying Dremio using Golang and ODBC driver"
date: 2020-04-14 20:10:00 +0300
tags: Dremio, Odbc , Golang, Postgres, Linux
category: Tutorial
author: Poonam Agarwal
---

	Dremio can be accessed via golang or other programming language. Here we are moving ahead by having two docker containers one of dremio and other of the database postgres.
	To access it via code or isql you need to have Dremio odbc Driver and unixODBC driver installed as we are in Linux environment (For windos it is not required).
		
	And that is it. Let follow the steps below.

### Prerequisites:
	Docker installed

### Run postgres and dremio container
	Postgres:
	docker run --name some-postgres -p 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword -d postgres

	Dremio:
	docker run -d -p 9047:9047 -p 31010:31010 -p 45678:45678 dremio/dremio-oss

### Configure dremio to get the postgress data
	* Go to http://127.0.0.1/9047 to access dremio
	* Create account in dremio by providing the details, and do remember username and password.
	* Go to → add source → select postgres → provide the required details
	* To get postgres ip you can go inside the container and do cat /etc/hosts and you can find the ip

<div>

<figure>
<img src="{{ site.github.url }}/media/img/dremio-postgres1.png" />
<figcaption>Login Details</figcaption>
</figure>

</div>

	Once connected follow the steps below.


#### Install Dremio odbc driver

	$ wget https://download.dremio.com/odbc-driver/1.4.2.1003/dremio-odbc-1.4.2.1003-1.x86_64.rpm
	$ sudo yum localinstall dremio-odbc-1.4.2.1003-1.x86_64.rpm

#### Install go
	$ wget https://dl.google.com/go/go1.13.linux-amd64.tar.gz
	$ sudo tar -C /usr/local -xzf go1.13.linux-amd64.tar.gz
	$ vi ~/.bash_profile
    	export PATH=$PATH:/usr/local/go/bin
    $ source ~/.bash_profile 

#### Setting up odbc.ini and odbcinst.ini file
	Copy and Paste the below file to your odbc.ini file and odbcinst.ini respectively. You can find the location of ini file by the following command:

	$ odbcinst -j

	$ sudo vi /etc/odbc.ini
			[ODBC Data Sources]
			DREMIO=Dremio ODBC Driver 64-bit

			[DREMIO]
			Description=Dremio ODBC Driver (64-bit) DSN
			Driver=/opt/dremio-odbc/lib64/libdrillodbc_sb64.so
			ConnectionType=Direct
			HOST=192.168.1.4
			PORT=31010
			DATABASE=mydb
			ZKQuorum=
			ZKClusterID=
			AuthenticationType=Plain
			DelegationUID=
			AdvancedProperties=CastAnyToVarchar=true;HandshakeTimeout=5;QueryTimeout=180;TimestampTZDisplayTimezone=utc;NumberOfPrefetchBuffers=5;
			Catalog=DREMIO
			Schema=
			SSL=0
			DisableHostVerification=0
			DisableCertificateVerification=0
			TrustedCerts=/opt/dremio-odbc/lib64/cacerts.pem
			UseSystemTrustStore=0
			UseExactTLSProtocolVersion=0
			Min_TLS=
			TLSProtocol=

	$ sudo vi /etc/odbcinst.ini
			[Dremio ODBC Driver 64-bit]
			Description=Dremio ODBC Driver(64-bit)
			Driver=/opt/dremio-odbc/lib64/libdrillodbc_sb64.so
			UsageCount=1

#### Install unixODBC driver

	$ wget ftp://ftp.unixodbc.org/pub/unixODBC/unixODBC-2.3.7.tar.gz
	$ gunzip unixODBC-2.3.7.tar.gz
	$ tar xvf unixODBC*.tar
	$ cd unixODBC-2.3.7/
	$ ./configure  --prefix=/usr/local/unixODBC
	$ make
	$ sudo make install
	$ export CGO_CFLAGS="-I/usr/local/unixODBC/include"
    $ export CGO_LDFLAGS="-L/usr/local/unixODBC/lib"
	$ go get github.com/alexbrainman/odbc
	$ go run test.go
			package main
			import (
			_ "github.com/alexbrainman/odbc"
			"database/sql"
			"log"
			"fmt"
			)
			func main() {
			    var (
			        ID   int
			        NAME string
				   AGE int
				   ADDRESS string
				   SALARY float64
			    )
			    db, err := sql.Open("odbc","DSN=Dremio;UID=<dremio login username>;PWD=<dremio password>")
			    if err != nil {
			                log.Fatal(err)
			    }
			    rows, err := db.Query("SELECT * from test.company")
			    if err != nil {
			            log.Fatal(err)
			    }
			    if err != nil {
			        fmt.Println("Unable to Query :( ", err)
			    }
			    defer rows.Close()
			    for rows.Next() {
			        err := rows.Scan(&ID,&NAME,&AGE,&ADDRESS,&SALARY )
			        if err != nil {
			            fmt.Println("Error when parsing :( ", err)
			        }
			 
			        fmt.Println(ID, NAME, AGE, ADDRESS, SALARY)
			    }
			    err = rows.Err()
			    if err != nil {
			        fmt.Println(err)
			    }
				defer db.Close()
			}

