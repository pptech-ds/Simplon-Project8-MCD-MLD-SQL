# Table of Contents 
- [Project-Specification](#Project-Specification) 
- [CDM-MCD-in-French-based-on-inputs](#CDM-MCD-in-French-based-on-inputs) 
- [LDM-MLD-in-French-generated-from-CDM](#LDM-MLD-in-French-generated-from-CDM) 
- [SQL-Script-Generated-From-CDM](#SQL-Script-Generated-From-CDM) 
- [SQL-database-in-PhpMyAdmin](#SQL-database-in-PhpMyAdmin) 
- [User-Case-Simulation](#User-Case-Simulation) 
- [SQL-Request-To-Show-Advertisements-On-Page](#SQL-Request-To-Show-Advertisements-On-Page) 

# Project-Specification
For a group of real estate agencies, you have to model the database with the following inputs:  
  - Agencies have negotiators who find and register property for sale.  
  - These properties can be of different types: houses, flats, offices etc ...
  - Created advertisements contain obvious data like title, description, price etc ...
  - All advertisements have medias like photo or videos.
  - You can complete with any other informations.  

# CDM-Conceptual-Data-Model-based-on-inputs  
![image](https://user-images.githubusercontent.com/61125395/121778862-38e68b80-cb99-11eb-8be4-08908e614514.png)          
- Entities with their attributes:
  - USER:  
  ![image](https://user-images.githubusercontent.com/61125395/121778967-b3afa680-cb99-11eb-8dd4-e2bced77ad44.png)  
  - USER_CATEGORY:  
  ![image](https://user-images.githubusercontent.com/61125395/121778992-d641bf80-cb99-11eb-9589-938ed0454069.png)  
  - AGENCY:  
  ![image](https://user-images.githubusercontent.com/61125395/121779017-eb1e5300-cb99-11eb-9453-2914cb405bf6.png)  
  - ADVERTISEMENT:  
  ![image](https://user-images.githubusercontent.com/61125395/121779036-fd988c80-cb99-11eb-8446-38ba11f4d4ca.png)  
  - PROPERTY:  
  ![image](https://user-images.githubusercontent.com/61125395/121779059-1d2fb500-cb9a-11eb-8efd-2060869b199f.png)  
  - PROPERTY_CATEGORY:  
  ![image](https://user-images.githubusercontent.com/61125395/121779073-2e78c180-cb9a-11eb-949a-1d35afac3b31.png)  
  - PROPERTY_MEDIA:  
  ![image](https://user-images.githubusercontent.com/61125395/121779096-43edeb80-cb9a-11eb-8a45-685fc9d1fb51.png)  
  
- Some explanations about entity relationships and cardinalities:  
  - USER(1,n)->(user is)->(0,n)USER_CATEGORY: About categories, we can have "manager", "negociator", "agent", "secretary" or "publisher". Here we define user roles, one user need to have at least one role, but he can have more than one, for example in a small agency the manager can be also the negociator, the agent and the one who publishes advertisements. For user categories, it's 0 to many because we can empty categories until it will be filled, like secretary for example.
  - USER(0,n)->(user belongs to)->(1,n)AGENCY: One user can belong to 0 or many agencies because the user can be a freelancer and he can work with any companies, the agency need to have at least one user, I mean the manager, the reponsible of the agency, an empty agency without anyone to manage it doesn't exist.  
  - USER(0,n)->(user publishes)->(1,1)ADVERTISEMENT: Advertisements will be published by users in specific category "publisher", one user can publish 0 to many advertisements, but one advertisement need to be published by one and only user.  
  - USER(0,n)->(user updates)->(0,n)ADVERTISEMENT: Advertisements can be updated or not by any users in category "publisher".  
  - USER(0,n)->(user negociates)->(1,1)PROPERTY: User in category "negociator" can negaviate property, user can negociate 0 to many properties, but one property need to be negociated by one and only one user.  
  - ADVERTISEMENT(1,n)->(advertisement contains)->(0,1)PROPERTY: An advertisement need to contain at least one property, but it can contain more than one, for example if you have a flat with a parking place or garage, they can be defined by 2 different properties but they can be sold in the same lot. The property can be affiliated to nothing waiting an advertisement, but it can be affiliated to only one an advertisement to avoid any issue in selling processes.  
  - PROPERTY(0,n)->(property contains)->(1,1)PROPERTY_MEDIA: Property can contain 0 to many medias, medias can be photo, videos or virtual visits. But one media is affiliated to one and only one property.  
  - PROPERTY(1,1)->(property is)->(0,n)PROPERTY_CATEGORY: A property need to be in one and only one specific category, it can a flat, house, parking, garage, office, castle etc ... One category can have 0 to many properties.  
 
 
# LDM-Logical-Data-Model-generated-from-CDM
![image](https://user-images.githubusercontent.com/61125395/121779740-86fd8e00-cb9d-11eb-9311-03ec6d611a1f.png)   
We finally have 10 entities, we had 7 entities in our CDM, some relationships have be changed to entities with foreign keys.  

# SQL-Script-Generated-From-CDM  
I had to complete the last table myself beacause of limitation from JMerise free version.  
```SQL
#------------------------------------------------------------
#        Script MySQL.
#------------------------------------------------------------


#------------------------------------------------------------
# Table: USER
#------------------------------------------------------------

CREATE TABLE USER(
        idUser          Int  Auto_increment  NOT NULL ,
        userFirstname   Varchar (150) NOT NULL ,
        userLastname    Varchar (150) NOT NULL ,
        userEmail       Varchar (150) NOT NULL ,
        userPassword    Varchar (50) NOT NULL ,
        userPhoneNumber Int NOT NULL ,
        userCountry     Varchar (150) NOT NULL ,
        userAddress     Varchar (150) NOT NULL ,
        userCity        Varchar (150) NOT NULL ,
        userZipCode     Int NOT NULL ,
        userSalary      Float NOT NULL ,
        userCommission  Float NOT NULL
	,CONSTRAINT USER_PK PRIMARY KEY (idUser)
)ENGINE=InnoDB;


#------------------------------------------------------------
# Table: AGENCY
#------------------------------------------------------------

CREATE TABLE AGENCY(
        idAgency                 Int  Auto_increment  NOT NULL ,
        agencyName               Varchar (150) NOT NULL ,
        agencyPhoneNumber        Int NOT NULL ,
        agencyEmail              Varchar (50) NOT NULL ,
        agencyCountry            Varchar (150) NOT NULL ,
        agencyAddress            Varchar (150) NOT NULL ,
        agencyCity               Varchar (150) NOT NULL ,
        agencyZipCode            Int NOT NULL ,
        agencyRegistrationNumber Varchar (150) NOT NULL
	,CONSTRAINT AGENCY_PK PRIMARY KEY (idAgency)
)ENGINE=InnoDB;


#------------------------------------------------------------
# Table: ADVERTISEMENT
#------------------------------------------------------------

CREATE TABLE ADVERTISEMENT(
        idAdvertisement          Int  Auto_increment  NOT NULL ,
        advertisementTitle       Varchar (150) NOT NULL ,
        advertisementDescription Varchar (500) NOT NULL ,
        advertisementPrice       Float NOT NULL ,
        userPlublishDate         TimeStamp NOT NULL ,
        idUser                   Int NOT NULL
	,CONSTRAINT ADVERTISEMENT_PK PRIMARY KEY (idAdvertisement)

	,CONSTRAINT ADVERTISEMENT_USER_FK FOREIGN KEY (idUser) REFERENCES USER(idUser)
)ENGINE=InnoDB;


#------------------------------------------------------------
# Table: USER_CATEGORY
#------------------------------------------------------------

CREATE TABLE USER_CATEGORY(
        idUserCategory   Int  Auto_increment  NOT NULL ,
        userCategoryName Varchar (255) NOT NULL
	,CONSTRAINT USER_CATEGORY_PK PRIMARY KEY (idUserCategory)
)ENGINE=InnoDB;


#------------------------------------------------------------
# Table: PROPERTY_CATEGORY
#------------------------------------------------------------

CREATE TABLE PROPERTY_CATEGORY(
        idPropertyCategory   Int  Auto_increment  NOT NULL ,
        propertyCategoryName Varchar (150) NOT NULL
	,CONSTRAINT PROPERTY_CATEGORY_PK PRIMARY KEY (idPropertyCategory)
)ENGINE=InnoDB;


#------------------------------------------------------------
# Table: PROPERTY
#------------------------------------------------------------

CREATE TABLE PROPERTY(
        idProperty                 Int  Auto_increment  NOT NULL ,
        propertyName               Varchar (150) NOT NULL ,
        propertyContactFirstname   Varchar (150) NOT NULL ,
        propertyContactLastname    Varchar (150) NOT NULL ,
        propertyContactPhoneNumber Int NOT NULL ,
        propertyContactEmail       Varchar (150) NOT NULL ,
        propertyCountry            Varchar (150) NOT NULL ,
        propertyAddress            Varchar (150) NOT NULL ,
        propertyCity               Varchar (150) NOT NULL ,
        propertyZipCode            Int NOT NULL ,
        propertyDetails            Varchar (1000) NOT NULL ,
        userNegociationDate        TimeStamp NOT NULL ,
        idAdvertisement            Int ,
        idPropertyCategory         Int NOT NULL ,
        idUser                     Int NOT NULL
	,CONSTRAINT PROPERTY_PK PRIMARY KEY (idProperty)

	,CONSTRAINT PROPERTY_ADVERTISEMENT_FK FOREIGN KEY (idAdvertisement) REFERENCES ADVERTISEMENT(idAdvertisement)
	,CONSTRAINT PROPERTY_PROPERTY_CATEGORY0_FK FOREIGN KEY (idPropertyCategory) REFERENCES PROPERTY_CATEGORY(idPropertyCategory)
	,CONSTRAINT PROPERTY_USER1_FK FOREIGN KEY (idUser) REFERENCES USER(idUser)
)ENGINE=InnoDB;


#------------------------------------------------------------
# Table: PROPERTY_MEDIA
#------------------------------------------------------------

CREATE TABLE PROPERTY_MEDIA(
        idPropertyMedia  Int  Auto_increment  NOT NULL ,
        propertyMediaUrl Varchar (150) NOT NULL ,
        idProperty       Int NOT NULL
	,CONSTRAINT PROPERTY_MEDIA_PK PRIMARY KEY (idPropertyMedia)

	,CONSTRAINT PROPERTY_MEDIA_PROPERTY_FK FOREIGN KEY (idProperty) REFERENCES PROPERTY(idProperty)
)ENGINE=InnoDB;


#------------------------------------------------------------
# Table: user belongs to
#------------------------------------------------------------

CREATE TABLE user_belongs_to(
        idAgency Int NOT NULL ,
        idUser   Int NOT NULL
	,CONSTRAINT user_belongs_to_PK PRIMARY KEY (idAgency,idUser)

	,CONSTRAINT user_belongs_to_AGENCY_FK FOREIGN KEY (idAgency) REFERENCES AGENCY(idAgency)
	,CONSTRAINT user_belongs_to_USER0_FK FOREIGN KEY (idUser) REFERENCES USER(idUser)
)ENGINE=InnoDB;


#------------------------------------------------------------
# Table: user is
#------------------------------------------------------------

CREATE TABLE user_is(
        idUserCategory Int NOT NULL ,
        idUser         Int NOT NULL
	,CONSTRAINT user_is_PK PRIMARY KEY (idUserCategory,idUser)

	,CONSTRAINT user_is_USER_CATEGORY_FK FOREIGN KEY (idUserCategory) REFERENCES USER_CATEGORY(idUserCategory)
	,CONSTRAINT user_is_USER0_FK FOREIGN KEY (idUser) REFERENCES USER(idUser)
)ENGINE=InnoDB;


#------------------------------------------------------------
# Table: user updates
#------------------------------------------------------------

CREATE TABLE user_updates(
        idAdvertisement Int NOT NULL ,
        idUser          Int NOT NULL ,
        userUpdateDate  TimeStamp NOT NULL
	,CONSTRAINT user_updates_PK PRIMARY KEY (idAdvertisement,idUser)

	,CONSTRAINT user_updates_ADVERTISEMENT_FK FOREIGN KEY (idAdvertisement) REFERENCES ADVERTISEMENT(idAdvertisement)
	,CONSTRAINT user_updates_USER0_FK FOREIGN KEY (idUser) REFERENCES USER(idUser)
)ENGINE=InnoDB;
```

# SQL-database-in-PhpMyAdmin  
Using the previous script, we can generate the SQL database in MySQL:  
![image](https://user-images.githubusercontent.com/61125395/121779888-21f66800-cb9e-11eb-8237-e87485ad4bb0.png)     

# User-Case-Simulation  
By adding step by step data in our database we can try to simulate a user case:  
1- Step1: Let's add some users to manage advertisments:  
```SQL
INSERT INTO `user` (`idUser`, `userFirstname`, `userLastname`, `userEmail`, `userPassword`, `userPhoneNumber`, `userCountry`, `userAddress`, `userCity`, `userZipCode`, `userSalary`, `userCommission`) VALUES (NULL, 'stephane', 'plazza', 'stephane.plazza@perso.com', '123456', '0123456789', 'france', '1 rue de la chance', 'paris', '75016', '50000', '0.05');
```
![image](https://user-images.githubusercontent.com/61125395/121780065-f32cc180-cb9e-11eb-9ae7-9beceea2d792.png)    

2- Step2: Let's add some user categories to asign roles to users:
```SQL
INSERT INTO `user_category` (`idUserCategory`, `userCategoryName`) VALUES (NULL, 'manager');
```
![image](https://user-images.githubusercontent.com/61125395/121780126-3b4be400-cb9f-11eb-9b0e-af241e1f779b.png)    

3- Step3: Let's link users to roles in table "user_is":
```SQL
INSERT INTO `user_is` (`idUserCategory`, `idUser`) VALUES ('1', '1');
```
![image](https://user-images.githubusercontent.com/61125395/121780204-9382e600-cb9f-11eb-9856-327604f138e5.png)    

4- Step4: Let's add some agencies:  
```SQL
INSERT INTO `agency` (`idAgency`, `agencyName`, `agencyPhoneNumber`, `agencyEmail`, `agencyCountry`, `agencyAddress`, `agencyCity`, `agencyZipCode`, `agencyRegistrationNumber`) VALUES (NULL, 'plazza immo', '0123456789', 'plazza.info@perso.com', 'france', '1 rue de la decouverte', 'paris', '75001', '15898zae547ezf');
```
![image](https://user-images.githubusercontent.com/61125395/121780313-05f3c600-cba0-11eb-92c1-d4ffe75c3b76.png)  

5- Step5: Let's link users to agencies:  
```SQL
INSERT INTO `user_belongs_to` (`idAgency`, `idUser`) VALUES ('3', '4');
```
![image](https://user-images.githubusercontent.com/61125395/121780390-5703ba00-cba0-11eb-8ea1-c4d1d380262c.png)  

6- Step6: Let's add some property categories: 
```SQL
INSERT INTO `property_category` (`idPropertyCategory`, `propertyCategoryName`) VALUES (NULL, 'flat');
```
![image](https://user-images.githubusercontent.com/61125395/121780459-a944db00-cba0-11eb-8d77-0c1bff4913fc.png)   

7- Step7: Let's add some properties:  
```SQL
INSERT INTO `property` (`idProperty`, `propertyName`, `propertyContactFirstname`, `propertyContactLastname`, `propertyContactPhoneNumber`, `propertyContactEmail`, `propertyCountry`, `propertyAddress`, `propertyCity`, `propertyZipCode`, `propertyDetails`, `userNegociationDate`, `idAdvertisement`, `idPropertyCategory`, `idUser`) VALUES (NULL, 'Appartement 3 pieces paris 13', 'jacque', 'martin', '0123456789', 'dqsd@ssg.com', 'france', 'sqdqsd', 'paris', '75005', 'qsdqsd', '2021-06-02 17:07:49', NULL, '1', '4');
```
![image](https://user-images.githubusercontent.com/61125395/121780596-5c153900-cba1-11eb-9e2a-bbea6c7602f1.png)    

8- Step8: Let's add some property medias:  
```SQL
INSERT INTO `property_media` (`idPropertyMedia`, `propertyMediaUrl`, `idProperty`) VALUES (NULL, 'https://album.photo.com/img1.jpg', '1');
```
![image](https://user-images.githubusercontent.com/61125395/121780651-9e3e7a80-cba1-11eb-9f24-ae7e1c5a5988.png)    

9- Step9: Let's add some advertisements:  
```SQL
INSERT INTO `advertisement` (`idAdvertisement`, `advertisementTitle`, `advertisementDescription`, `advertisementPrice`, `userPlublishDate`, `idUser`) VALUES (NULL, 'Appartement 3 pieces sur Paris', 'qsdqsd', '300000', '2021-06-02 17:14:31', '2');
```
![image](https://user-images.githubusercontent.com/61125395/121780717-e2317f80-cba1-11eb-8448-e9a0e30babe6.png)    

10- Step10: Let's simulate an update from a user:  
```SQL
INSERT INTO `user_updates` (`idAdvertisement`, `idUser`, `userUpdateDate`) VALUES ('1', '5', '2021-06-19 17:17:21');
```
![image](https://user-images.githubusercontent.com/61125395/121780775-21f86700-cba2-11eb-92e7-c1e152f640ed.png)  


# SQL-Request-To-Show-Advertisements-On-Page  
Let's assume a previous request has been done to build the attribute "description" of table "advertisments", and we want to aggregate all other informations like user who has publised the advertisement. Here is the SQL request and result:  
```SQL
SELECT `advertisementTitle`,`advertisementDescription`,`advertisementPrice`,`agencyPublishDate`,`userRegisterDate`, user.userFirstname, user.userLastname, user.userEmail
FROM `advertisement` 
INNER JOIN user
WHERE advertisement.idUser = user.idUser
```
![image](https://user-images.githubusercontent.com/61125395/121764434-1f672480-cb44-11eb-8697-9fabb79d0deb.png)

  

