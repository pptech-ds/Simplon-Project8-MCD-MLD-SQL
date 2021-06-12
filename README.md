# Project-Specification
For a group of real estate agencies, you have to model the database with the following inputs:  
  - Agencies have negotiators who find and register property for sale.  
  - These properties can be of different types: houses, flats, offices etc ...
  - Created advertisements contain obvious data like title, description, price etc ...
  - All advertisements have medias like photo or videos.
  - You can complete with any other informations.  

# CDM (MCD in French) based on inputs:  
![image](https://user-images.githubusercontent.com/61125395/121762560-effdeb00-cb36-11eb-9581-57f013fcd43f.png)          
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
  ![image](https://user-images.githubusercontent.com/61125395/121761972-8203f480-cb33-11eb-9463-2f70d8f3b3f3.png)  
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

# LDM (MLD in French) generated from CDM:
![image](https://user-images.githubusercontent.com/61125395/121762761-43bd0400-cb38-11eb-8e9f-ca7d1f47edca.png)  
We finally have 10 entities, we had 7 entities in our CDM, some relationships have be changed to entities with foreign keys.  



