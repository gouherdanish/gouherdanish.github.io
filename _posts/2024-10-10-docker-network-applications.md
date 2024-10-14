---
layout: post
title: "Docker: Handling multi-containered apps with Docker Network"
date: 2024-10-10
tags: ["Software Development"]
---

Docker compose provides a declarative syntax which helps manage deployments efficiently

---
### Background

- Previously, in [this](https://gouherdanish.github.io/2024/09/25/low-lying-areas-mapping.html) blog, we created a simple app to locate low-lying areas in Bangalore
- Also, in [this](https://gouherdanish.github.io/2024/10/07/dockerfile.html) article, we created a Dockerfile for the same and built its image and ran it locally as a container

---
### New Requirements
- Let's say, we get new requirements to add couple of features into our app
    - Keep a log for the search history in our app so that we know how many times each villages are searched by users
    - Keep a log for the last searched village in our app so that the user should have to search for his village if the page is refreshed

---
### Database to the Rescue
- The obvious solution to this is to use a database
- We can use MongoDB for this since most users will be searching for their villages and thus DB writes will be more. MongoDB is optimized for write-heavy needs
- Also, MongoDB is easy to use with its Mongo Express UI

---
### Database Setup
- MongoDB can be setup using Docker very easily

**Step 1 - Pull images**
- Pull `mongo` and `mongo-express` images from Dockerhub

```
docker pull mongo
docker pull mongo-express
```

**Step 2 - Create new Docker Network**

```
docker network create mongo-network
```

**Step 3 - Run Containers**
- Start MongoDB container using `docker run` command

```
docker run -d \                                                                     
-p 27017:27017 \ 
-e MONGO_INITDB_ROOT_USERNAME=admin \
-e MONGO_INITDB_ROOT_PASSWORD=password \
--network mongo-network \
--name mongodb \
mongo
```

- Next, start Mongo Express container
```
docker run -d \                                                                     
-p 8081:8081 \
--name mongoui \
--network mongo-network \
-e ME_CONFIG_MONGODB_SERVER=mongodb \
-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
mongo-express
```

Note
- Make sure to run both in same network so that the container can communicate natively

**Step 4 - Check containers**
- Both containers should be running

```
docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS          PORTS                      NAMES
2325792727d9   mongo-express     "/sbin/tini -- /dock…"   2 days ago       Up 2 days       0.0.0.0:8081->8081/tcp     mongoui
e29bf436c061   mongo             "docker-entrypoint.s…"   2 days ago       Up 2 days       0.0.0.0:27017->27017/tcp   mongodb
```

**Step 5 - Check Mongo UI**
- Open 0.0.0.0:8081 on browser for Mongo Express UI

<img src="{{site.url}}/images/docker/mongoui_0.png">

**Step 6 - Create Document and Collection on MongoUI**

<img src="{{site.url}}/images/docker/mongoui.png">

<img src="{{site.url}}/images/docker/mongoui_1.png">

---

### Implementing Requirements

- Find the village whose last searched flag is True
    - If 
- Upsert the current village into the collection by incrementing its search counter by 1 and setting its last searched flag to True
- Update the last searched flag to False for all other villages in the collection 

```
# app.py
class App:
    ...
    def fetch(self):
        """
        Queries the MongoDB collection and dumps all the documents in a dataframe
        Hides dataframe index
        """
        df = pd.DataFrame(self.user_requests_collection.find()).drop('_id',axis=1)
        return df.style.hide_index().to_html() if len(df) != 0 else "None"
    
    def persist(self,village):
        """
        Performs two tasks
        - Upserts the current village into the collection by incrementing its search counter to 1 and making it the last searched item
        - Updates the last searched flag to False for all other villages in the collection 
        """
        self.user_requests_collection.update_one(
            { "village": village },
            { "$inc": { "count": 1 }, "$set": { "last": True }},
            upsert = True
        )
        self.user_requests_collection.update_many(
            { "village": {"$ne": village }},
            { "$set": { "last": False } }
        )
    
    def search_history(self):
        """
        Creates the search history as a dataframe table in the sidebar
        """
        st.sidebar.markdown("### Search History")
        st.sidebar.write(self.fetch(),unsafe_allow_html=True)

    def last_searched_village(self):
        """
        Queries MongoDB collection to find the village whose last searched flag is True
        """
        last_searched_village = self.user_requests_collection.find_one({"last":True})
        return last_searched_village if last_searched_village else {"village": None}
```

