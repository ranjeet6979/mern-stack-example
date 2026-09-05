# Docker Steps

## Step 1: Clone the repository

```bash
git clone https://github.com/ranjeet6979/mern-stack-example.git
cd mern-stack-example
```

---

## Step 2: Build frontend image

Go to frontend:

```bash
cd mern/client
```

Build image:

```bash
docker build -t mern-client .
```

<img width="1418" height="349" alt="image" src="https://github.com/user-attachments/assets/880adf3e-54e6-4a74-befc-028f0ba11243" />


Go back:

```bash
cd ../..
```

---

## Step 3: Build backend image

Go to backend:

```bash
cd mern/server
```

Build image:

```bash
docker build -t mern-server .
```

<img width="1425" height="351" alt="image" src="https://github.com/user-attachments/assets/46208474-5889-4143-a4cd-5e3b92109dee" />

Go back:

```bash
cd ../..
```

Check images:

```bash
docker images
```
<img width="908" height="49" alt="image" src="https://github.com/user-attachments/assets/b0ceb13c-1f5e-480c-8276-e62bdc2e1658" />

---

## Step 4: Create Docker network

```bash
docker network create mern-example-net
```

Check:

```bash
docker network ls
```

<img width="554" height="34" alt="image" src="https://github.com/user-attachments/assets/d96fd60c-46af-426f-9147-801271f3ff51" />


---

## Step 5: Run MongoDB

Create a volume first so MongoDB data persists:

```bash
docker volume create mongo_data
```

<img width="555" height="38" alt="image" src="https://github.com/user-attachments/assets/20d15da2-8eee-4d45-9f4f-2215227325ad" />


Run MongoDB:

```bash
docker run -d \
  --name mongodb \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=admin123 \
  --network mern-example-net \
  -v mongo_data:/data/db \
  mongo:8
```

<img width="470" height="137" alt="image" src="https://github.com/user-attachments/assets/f6a0b2a2-5d3a-44c1-b548-4179af140fd1" />


Check:

```bash
docker ps
```
<img width="1090" height="31" alt="image" src="https://github.com/user-attachments/assets/384c4a15-3016-431a-83a8-c60ae1506dfb" />

---

## Step 6: Run backend

Run:

```bash
docker run -d \
  --name mern-server \
  --network mern-example-net \
  -p 5050:5050 \
  -e ATLAS_URL="mongodb://admin:admin123@mongodb:27017/employees?authSource=admin" \
  mern-server
```
<img width="603" height="108" alt="image" src="https://github.com/user-attachments/assets/022ea477-6ee7-4c67-b67b-6c05a277be0f" />

Check logs:

```bash
docker logs mern-server
```

<img width="531" height="168" alt="image" src="https://github.com/user-attachments/assets/67b37193-4e0f-42ab-b6d4-cc0dfce72712" />

---

## Step 7: Run frontend

```bash
docker run -d \
  --name mern-client \
  --network mern-example-net \
  -p 5173:5173 \
  mern-client
```

<img width="474" height="95" alt="image" src="https://github.com/user-attachments/assets/38d90f7d-0301-49f6-b707-a1e1d61df6c3" />

Check:

```bash
docker ps
```

<img width="1123" height="62" alt="image" src="https://github.com/user-attachments/assets/2e00de82-6c7b-4a36-9c73-8f563c18914f" />

---

## Step 8: Open the application

Open in browser:

```text
http://localhost:5173
```
<img width="1446" height="314" alt="image" src="https://github.com/user-attachments/assets/b7790e3c-90bc-4720-9ea7-0bb81177538c" />

---

## Step 9: Crate employee record

<img width="1446" height="360" alt="image" src="https://github.com/user-attachments/assets/3959ad61-5025-4945-8f6c-1908b22cc0e9" />

## Step 9: Stop containers to verify volume persists data

```bash
docker stop mern-client mern-server mongodb
```
<img width="632" height="61" alt="image" src="https://github.com/user-attachments/assets/09f52800-4776-4cae-be3c-0a1fb2889ab8" />

Remove containers:

```bash
docker rm mern-client mern-server mongodb
```

<img width="619" height="62" alt="image" src="https://github.com/user-attachments/assets/35e7ba25-b667-463f-aca9-a73dc63b39d0" />

---

## Step 10: Repeat step 5 to 8 to verify data persist

<img width="606" height="333" alt="image" src="https://github.com/user-attachments/assets/506f1086-02ef-4493-bb68-1ac421e4ea56" />

<img width="1451" height="321" alt="image" src="https://github.com/user-attachments/assets/0c12665c-dde8-4cde-9eb0-c286eb5570ea" />


## Port Summary

| Component |  Port |
| --------- | ----: |
| Frontend  |  5173 |
| Backend   |  5050 |
| MongoDB   | 27017 |

After this manual setup works, **Step 2 of your exercise is complete**. Then we can create `docker-compose.yml` to replace all these `docker run` commands. Docker Compose is specifically designed to define services, networks, and volumes together.

# Docker compose steps

## Step 1: Go to root directory, stop running container if any

```bash
docker stop mern-client mern-server mongodb
```

```bash
docker rm mern-client mern-server mongodb
```

<img width="725" height="123" alt="image" src="https://github.com/user-attachments/assets/019013f2-b639-4a51-986d-9459a301eee4" />

Verify no running containers

```bash
docker ps
```

## Step 2: Run docker compose up -d

```bash
docker compose up -d
```

<img width="1445" height="358" alt="image" src="https://github.com/user-attachments/assets/ee21cd50-37cb-4524-9d47-f5f26b8df9b5" />

## Step 3: Create sample record to verify volume data exists

<img width="1445" height="359" alt="image" src="https://github.com/user-attachments/assets/0b70677a-dd2f-4a52-ba67-47ee7204e84f" />

## Step 4: Run docker compose down

```bash
docker compose down
```
<img width="553" height="89" alt="image" src="https://github.com/user-attachments/assets/e82816ce-cfdc-44ec-875f-13a38369542f" />

## Step 2: Run docker compose up -d again and data should persist


```bash
docker compose up -d
```

<img width="1448" height="342" alt="image" src="https://github.com/user-attachments/assets/1c60781b-aea1-4aef-8cbc-07c0794c8082" />

