# Developing a Web Application in a Container-Based Cloud Environment Using the Netflix Dataset

## 1. Project Description

This project focuses on the end-to-end development and deployment of a container-based web application
running on a cloud computing infrastructure. Within the scope of the project, a virtual machine was created
on AWS EC2 using Ubuntu Server, and Docker technology was used to run the web application and database
services as independent containers.

The application works with a dataset obtained from Kaggle, which is a real data source, and processes the
data on each user request, then stores it in a database. In this way, the project practically demonstrates both
application deployment in a cloud environment and how a data-driven web service can be developed.

## 2. Project Objective

The main objective of this project is to apply the cloud computing, virtualization, and container concepts
learned theoretically to a real-world scenario. With this project, it was aimed to gain hands-on experience in
the following areas;

Virtual machine management in a cloud environment,
Container-based application development,
Web application and database integration,
Security and network configurations

## 3. Real-World Use Cases

The architecture developed in this project forms the foundation of many systems that are widely used in real life.
In daily life, services such as;

E-commerce websites,
Movie and TV platforms,
News websites,
Social media applications

operate with a similar architecture.

For example, when a user refreshes a movie platform and different content is recommended each time,
retrieving this content from a database in the background and recording user interactions directly matches
the logic implemented in this project.


### Steps Followed During the Project

1. A suitable cloud infrastructure was selected for the project and an EC2 virtual machine was created on AWS.
2. Ubuntu Server was installed on the EC2 instance and basic system configurations were completed.
3. SSH key-based authentication was configured to provide secure access to the server.
4. Docker was installed to use a container-based architecture, and the environment required to run the services
    was prepared. _(_ **_sudo apt install docker.io -y , sudo systemctl start docker_** _)_
5. A dedicated Docker network was created to allow the web application and the database to communicate
    with each other. **_(docker network create appnet)_**
6. The MySQL database was started as a Docker container, and the necessary database and users were defined
    for the application. **_(docker run -d --name mysql-db --network appnet)_**
7. A Flask-based Python web application was developed and run as a Docker container.
    **_(docker run -d --name webapp --network appnet \_**
8. **_-p 80:5000 mywebapp)_**
9. A real dataset obtained from Kaggle was integrated into the application.
**_(import__kagglehub, path = kagglehub.dataset_download("shivamb/netflix-shows"))_**
10. The application selects random records from the dataset on each HTTP request and stores them in the MySQL
    database.
11. The inserted data was verified using SQL queries, and the application was tested to confirm it works correctly.
    **_(INSERT INTO netflix_titles (title, country, release_year), VALUES_**
    **_('Example Movie', 'USA', 2023);)_**
12. AWS Security Group rules and Linux firewall settings were configured to securely expose the application to the
    outside world. **_(sudo ufw allow 80 , sudo ufw allow 22)_**
13. The application was tested via a web browser and it was observed that the system works smoothly end-to-end.
    **_(curl [http://public_ipv4_address)](http://public_ipv4_address))_**


#### Problems Encountered and Solutions

During the project development process, various problems were encountered at both the infrastructure and
application layers. These issues became more apparent, especially during the integration process between the
Python-based web application and the MySQL database.

First, connection errors occurred while the Python application was connecting to the MySQL database. The main
reason for this was incorrect network configuration between Docker containers and the database service not being
fully ready before the application started. The issue was resolved by creating a dedicated Docker network for the
containers and ensuring that the application established the database connection using a retry–wait logic.

In addition, incompatibilities related to data types were observed during automatic table creation and data insertion
operations performed by the application. Errors were especially encountered in text length and date fields, and this
was fixed by redesigning the table schema.

Moreover, while writing data to the database on each HTTP request, concurrency and connection persistence problems
were experienced. This issue was resolved by opening and closing database connections in a controlled manner and
simplifying SQL queries.

Finally, SSH connections occasionally dropped and access issues occurred. This was caused by IP address changes and
resource limitations in cloud environments, and it was resolved by restarting the instance and using disk migration
methods.


## Lessons Learned

This project provided important gains not only as a web application development process but also as a way to
understand how cloud-based systems work holistically.

First, it was learned that code alone is not enough for an application to run in cloud environments; many components
such as networking, security, storage, and service management must be considered together. The work carried out on
AWS EC2 showed that virtual machine management and correct configuration of cloud resources are critical for
application stability.

Thanks to Docker technology, running the application and database services independently and making them portable
was an important experience. The container architecture simplified deployment processes and increased the
reproducibility of the system.

During the Python and SQL integration process, it was observed that communication between the application layer and
the database layer must be designed carefully. It was understood that incorrect queries or poor connection
management can affect the entire application.

It was also observed that working with a real dataset made the application more meaningful and helped demonstrate
the practical value of theoretical knowledge.


##### Possible Improvements

This project successfully presents a basic cloud-based web application architecture in its current form.
However, there are several areas for improvement in order to take the system to more advanced levels.

In future work, Docker Compose can be used to manage all services through a single configuration file. This can make
the setup process faster and less error-prone. In addition, the MySQL database can be migrated to a managed service
such as AWS RDS to simplify maintenance and backup processes.

Moreover, AWS services such as Elastic IP and Load Balancer can be used to increase the availability and continuity
of the application. Backing up datasets and application outputs on AWS S3 can also be considered as another
improvement that would increase the reliability of the system.

###### Conclusion

In conclusion, this project addressed cloud infrastructure, container architecture, and database integration—commonly
used in modern software systems—through a real-world scenario. The problems encountered during the project and the
solutions developed provided a valuable learning experience in cloud-based application development. In this respect,
the study represents an example that is applicable both academically and within the industry.
