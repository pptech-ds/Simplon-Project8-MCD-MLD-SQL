# Table of Contents 
- [Project-Specification](#Project-Specification) 
- [CDM-MCD-in-French-based-on-inputs](#CDM-MCD-in-French-based-on-inputs) 
- [LDM-MLD-in-French-generated-from-CDM](#LDM-MLD-in-French-generated-from-CDM) 
- [SQL-database-in-PhpMyAdmin](#SQL-database-in-PhpMyAdmin) 
- [User-Case-Simulation](#User-Case-Simulation) 
- [SQL-request-to-show-advertisements-on-page](#SQL-request-to-show-advertisements-on-page) 

# Project-Specification
For a group of real estate agencies, you have to model the database with the following inputs:  
  - Agencies have negotiators who find and register property for sale.  
  - These properties can be of different types: houses, flats, offices etc ...
  - Created advertisements contain obvious data like title, description, price etc ...
  - All advertisements have medias like photo or videos.
  - You can complete with any other informations.  

# CDM-MCD-in-French-based-on-inputs  
![image](https://user-images.githubusercontent.com/61125395/121763697-6e11c000-cb3e-11eb-9301-9a5f632cb7db.png)          
- Entities with their attributes:
  - USER:  
  ![image](https://user-images.githubusercontent.com/61125395/121761898-0144f880-cb33-11eb-822a-e45264064d59.png)  
  - USER_CATEGORY: 
  ![image](https://user-images.githubusercontent.com/61125395/121761918-1ae64000-cb33-11eb-82a9-205b16a21f0a.png)  
  - AGENCY:  
  ![image](https://user-images.githubusercontent.com/61125395/121761932-3cdfc280-cb33-11eb-8838-59a1195dd8e1.png)  
  - ADVERTISEMENT:  
  ![image](https://user-images.githubusercontent.com/61125395/121761946-4b2dde80-cb33-11eb-932c-0032ef99f29f.png)  
  - PROPERTY:  
  ![image](https://user-images.githubusercontent.com/61125395/121763890-b41b5380-cb3f-11eb-8726-c9cab4215c13.png)  
  - PROPERTY_CATEGORY:  
  ![image](https://user-images.githubusercontent.com/61125395/121761980-921bd400-cb33-11eb-9081-0439d8db3df5.png)  
  - PROPERTY_MEDIA:  
  ![image](https://user-images.githubusercontent.com/61125395/121761987-9fd15980-cb33-11eb-8448-035b793f5562.png)  
  
- Some explanations about entity relationships and cardinalities:  
  - USER(1,n)->(user is)->(0,n)USER_CATEGORY: About categories, we can have "manager", "negociator", "agent", and "secretary. Here we define user roles, one user need to have at least one role, but he can have more than one, for example a in small agency the manager can be also the negociator and the agent.  
  - USER(0,n)->(user belongs to)->(0,n)USER_CATEGORY: One user can belong to 0 or many agencies if he is a freelancer, the agency can have 0 user waiting the asignation.  
  - AGENCY(0,n)->(agency publishes)->(1,1)ADVERTISEMENT: The agency can publish 0 to many advertisements, but an advertisement need to be publised by one and only one agency. 
  - ADVERTISEMENT(1,n)->(advertisement contains)->(0,1)PROPERTY: An advertisement need to contain at least one property, but can contain more than one, for example if you have a flat with a parking place or garage, they can be defined by 2 different properties but they can be sold in the same lot. The property can be affiliated to nothing waiting an advertisement, but it can be affiliated to only one an advertisement to avoid any issue in selling processes.  
  - PROPERTY(0,n)->(property contains)->(1,1)PROPERTY_MEDIA: Property can contain 0 to many medias, medias can be photo, videos or virtual visit. But one media is affiliated to one and only one property.  
  - PROPERTY(1,1)->(property is)->(0,n)PROPERTY_CATEGORY: A property need to be in one and only one specific category, it can a flat, house, parking, garage, office, castle etc ... One category can have many properties.  
  - USER(0,n)->(user registers)->(1,1)ADVERTISEMENT: One user can register 0 or many advertisments, but one advertisments can be registered by one and only one user.
  - USER(0,n)->(user updates)->(0,n)ADVERTISEMENT: One user can updates 0 to many times the advertisement, the advertisement can be updated 0 to many times.   

# LDM-MLD-in-French-generated-from-CDM
![image](https://user-images.githubusercontent.com/61125395/121762761-43bd0400-cb38-11eb-8e9f-ca7d1f47edca.png)  
We finally have 10 entities, we had 7 entities in our CDM, some relationships have be changed to entities with foreign keys.  

# SQL script generated from CDM:  
I had to complete the last table beacause of limitation from JMerise free version.  
```SQL
#------------------------------------------------------------
#        Script MySQL.
#------------------------------------------------------------


#------------------------------------------------------------
# Table: USER
#------------------------------------------------------------

CREATE TABLE USER(
        idUser         Int  Auto_increment  NOT NULL ,
        userFirstname  Varchar (150) NOT NULL ,
        userLastname   Varchar (150) NOT NULL ,
        userEmail      Varchar (150) NOT NULL ,
        userPassword   Varchar (50) NOT NULL ,
        userCountry    Varchar (150) NOT NULL ,
        userAddress    Varchar (150) NOT NULL ,
        userCity       Varchar (150) NOT NULL ,
        userZipCode    Int NOT NULL ,
        userSalary     Float NOT NULL ,
        userCommission Float NOT NULL
	,CONSTRAINT USER_PK PRIMARY KEY (idUser)
)ENGINE=InnoDB;


#------------------------------------------------------------
# Table: AGENCY
#------------------------------------------------------------

CREATE TABLE AGENCY(
        idAgency                 Int  Auto_increment  NOT NULL ,
        agencyName               Varchar (150) NOT NULL ,
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
        agencyPublishDate        TimeStamp NOT NULL ,
        userRegisterDate         TimeStamp NOT NULL ,
        idAgency                 Int NOT NULL ,
        idUser                   Int NOT NULL
	,CONSTRAINT ADVERTISEMENT_PK PRIMARY KEY (idAdvertisement)

	,CONSTRAINT ADVERTISEMENT_AGENCY_FK FOREIGN KEY (idAgency) REFERENCES AGENCY(idAgency)
	,CONSTRAINT ADVERTISEMENT_USER0_FK FOREIGN KEY (idUser) REFERENCES USER(idUser)
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
        idProperty           Int  Auto_increment  NOT NULL ,
        propertyCountry      Varchar (150) NOT NULL ,
        propertyAddress      Varchar (150) NOT NULL ,
        propertyCity         Varchar (150) NOT NULL ,
        propertyZipCode      Int NOT NULL ,
        propertyNbRooms      Int NOT NULL ,
        propertyNbKitchen    Int NOT NULL ,
        propertyNbBathroom   Int NOT NULL ,
        propertyNbWc         Int NOT NULL ,
        propertyNbLivingRoom Int NOT NULL ,
        idAdvertisement      Int ,
        idPropertyCategory   Int NOT NULL
	,CONSTRAINT PROPERTY_PK PRIMARY KEY (idProperty)

	,CONSTRAINT PROPERTY_ADVERTISEMENT_FK FOREIGN KEY (idAdvertisement) REFERENCES ADVERTISEMENT(idAdvertisement)
	,CONSTRAINT PROPERTY_PROPERTY_CATEGORY0_FK FOREIGN KEY (idPropertyCategory) REFERENCES PROPERTY_CATEGORY(idPropertyCategory)
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
![image](https://user-images.githubusercontent.com/61125395/121763217-7bc54680-cb3a-11eb-9901-4a9b4f6a7a0b.png)   

# User-Case-Simulation  
By adding step by step data in our database we can try to simulate a user case:  
1- Step1: Let's add some users to manage advertisments:  
```SQL
INSERT INTO `user` (`idUser`, `userFirstname`, `userLastname`, `userEmail`, `userPassword`, `userCountry`, `userAddress`, `userCity`, `userZipCode`, `userSalary`, `userCommission`) VALUES (NULL, 'stephane', 'plazza', 'stephane.plazza@perso.com', '123456', 'france', '1 rue de la chance', 'paris', '75016', '50000', '0.05'), (NULL, 'michel', 'durant', 'michel.durant@perso.com', '123456', 'france', '16 rue de la bouteille', 'paris', '75005', '40000', '0.04'), (NULL, 'sophia', 'dupont', 'sophia.dupont@perso.com', '123456', 'belgique', '20 bd voltaire', 'bruges', '8000', '45000', '0.05'), (NULL, 'vince', 'carter', 'vince.carter@perso.com', '123456', 'angleterre', '1 square garden', 'london', '533537', '40000', '0.05');
```
![image](https://user-images.githubusercontent.com/61125395/121763916-e2992e80-cb3f-11eb-98f3-1006124242ce.png)  

2- Step2: Let's add some user categories to asign roles to users:
```SQL
INSERT INTO `user_category` (`idUserCategory`, `userCategoryName`) VALUES (NULL, 'manager'), (NULL, 'negociator'), (NULL, 'agent'), (NULL, 'secretary');
```
![image](https://user-images.githubusercontent.com/61125395/121763921-f47ad180-cb3f-11eb-8b6d-4a363f60b478.png)  

3- Step3: Let's link users to roles in table "user_is":
```SQL
INSERT INTO `user_is` (`idUserCategory`, `idUser`) VALUES ('1', '1'), ('2', '1'), ('3', '4'), ('4', '1');
```
![image](https://user-images.githubusercontent.com/61125395/121763940-196f4480-cb40-11eb-8a44-973a15f518e4.png)  

4- Step4: Let's add some agencies:  
```SQL
INSERT INTO `agency` (`idAgency`, `agencyName`, `agencyCountry`, `agencyAddress`, `agencyCity`, `agencyZipCode`, `agencyRegistrationNumber`) VALUES (NULL, 'plazza immo', 'france', '1 bd des champs elysées', 'paris', '75001','12345gft258'), (NULL, 'la foret', 'france', '26 rue des invalides', 'bordeaux', '30072', '369poi147');
```
![image](https://user-images.githubusercontent.com/61125395/121763953-2855f700-cb40-11eb-9592-0f3228799d2e.png)  

5- Step5: Let's link users to agencies:  
```SQL
INSERT INTO `user_belongs_to` (`idAgency`, `idUser`) VALUES ('2', '3'), ('1', '1'), ('2', '4'), ('1', '4');
```
![image](https://user-images.githubusercontent.com/61125395/121763963-3f94e480-cb40-11eb-8dad-24bca54b68a4.png)  

6- Step6: Let's add some property categories: 
```SQL
INSERT INTO `property_category` (`idPropertyCategory`, `propertyCategoryName`) VALUES (NULL, 'flat'), (NULL, 'house'), (NULL, 'loft'), (NULL, 'parking'), (NULL, 'garden'), (NULL, 'castle');
```
![image](https://user-images.githubusercontent.com/61125395/121763974-59362c00-cb40-11eb-95c3-cf22316d7bc1.png)  

7- Step7: Let's add some properties:  
```SQL
INSERT INTO `property` (`idProperty`, `propertyCountry`, `propertyAddress`, `propertyCity`, `propertyZipCode`, `propertyNbRooms`, `propertyNbKitchen`, `propertyNbBathroom`, `propertyNbWc`, `propertyNbLivingRoom`, `idPropertyCategory`) VALUES (NULL, 'france', '1 rue de la joie', 'paris', '75005', '2', '1', '1', '1', '1', '3'), (NULL, 'france', '26 bd des fortunes', 'nantes', '44000', '4', '1', '2', '2', '1', '4');
```
![image](https://user-images.githubusercontent.com/61125395/121763983-67844800-cb40-11eb-9f2f-e3586d0134a8.png)  

8- Step8: Let's add some property medias:  
```SQL
INSERT INTO `property_media` (`idPropertyMedia`, `propertyMediaUrl`, `idProperty`) VALUES (NULL, 'https://photo.album.com/img6.jpg', '1'), (NULL, 'https://photo.album.com/img7.jpg', '2');
```
![image](https://user-images.githubusercontent.com/61125395/121763990-74a13700-cb40-11eb-8a70-5bc8ddff1be2.png)  

9- Step9: Let's add some advertisements:  
```SQL
INSERT INTO `advertisement` (`idAdvertisement`, `advertisementTitle`, `advertisementDescription`, `advertisementPrice`, `agencyPublishDate`, `userRegisterDate`, `idAgency`, `idUser`) VALUES (NULL, 'Appartement 2 pieces a vendre', 'fjdshfjkdsfk sfhj gjh', '200000', '2021-06-02 03:32:18', '2021-06-01 00:59:03', '2', '2'), (NULL, 'maison 4 pieces a vendre', 'fjkhsdjfkdhsk', '350000', '2021-06-05 03:32:18', '2021-06-03 03:27:55', '1', '3');
```
![image](https://user-images.githubusercontent.com/61125395/121763998-8aaef780-cb40-11eb-886f-55dfe47c6058.png)  

10- Step10: Let's simulate an update from a user:  
```SQL
INSERT INTO `user_updates` (`idAdvertisement`, `idUser`, `userUpdateDate`) VALUES ('1', '2', '2021-06-08 03:33:26');
```
![image](https://user-images.githubusercontent.com/61125395/121764004-95698c80-cb40-11eb-84c9-8c7043d827b1.png)  


# SQL-request-to-show-advertisements-on-page  
Let's assume a previous request has been done to build the attribute "description" of table "advertisments", and we want to aggregate all other informations like user who has publised the advertisement. Here is the SQL request and result:  
```SQL
SELECT `advertisementTitle`,`advertisementDescription`,`advertisementPrice`,`agencyPublishDate`,`userRegisterDate`, user.userFirstname, user.userLastname, user.userEmail
FROM `advertisement` 
INNER JOIN user
WHERE advertisement.idUser = user.idUser
```
![image](https://user-images.githubusercontent.com/61125395/121764434-1f672480-cb44-11eb-8697-9fabb79d0deb.png)

  

