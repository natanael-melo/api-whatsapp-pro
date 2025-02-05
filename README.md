</br>
<hr style="height: 5px;background: #007500;margin: 20px 0;box-shadow: 0px 3px 5px 0px rgb(204 204 204);">

<div align="center">

[![AWP Whatsapp Group](https://img.shields.io/badge/Group-WhatsApp-%2322BC18)](https://chat.whatsapp.com/Brm3eviNRSsADKz9tz5y1u)
[![CodeChat Whatsapp Group](https://img.shields.io/badge/Group-WhatsApp-%2322BC18)](https://chat.whatsapp.com/HyO8X8K0bAo0bfaeW8bhY5)
[![License](https://img.shields.io/badge/license-GPL--3.0-orange)](./LICENSE)
[![Support](https://img.shields.io/badge/Buy%20me-coffe-orange)](https://app.picpay.com/user/cleber.wilson.oliveira)
[![Support](https://img.shields.io/badge/Buy%20me%20coffe-pix-blue)](#pix-2b526ada-4ef4-4db4-bbeb-f60da2421fce)

</div>
  
<div align="center"><img src="./public/images/cover.png"></div>

## Project Structure

* [Look here](./PROJECT_STRUCTURE.md)

## WhatsApp-Api-NodeJs

This code is an implementation of [WhiskeySockets](https://github.com/WhiskeySockets/Baileys), as a RestFull Api service, which controls whatsapp functions.</br>
With this one you can create multiservice chats, service bots or any other system that uses whatsapp. With this code you don't need to know javascript for nodejs , just start the server and make the language requests that you feel most comfortable with.

## Infrastructure

### 1. Docker installation

* First, let's install Docker. Docker is a platform that allows us to quickly create, test and deploy applications in isolated environments called containers.

```sh
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ${USER}
```

* Restarting the Docker to apply the permissions

```sh
systemctl restart docker
```

### 2. Installing the database

* Creating database volume

```sh
sudo docker volume create --name postgres_data
```

* Running Postgres

```sh
sudo docker run -d \
--name postgres \
--network public_network \
--volume postgres_data:/var/lib/postgresql/data \
--env POSTGRES_PASSWORD=123456789 \
--env POSTGRES_INITDB_ARGS=--auth-host=scram-sha-256 \
-p 5432:5432 \
postgres:latest \
postgres -c max_connections=200
```

* Opening port in the firewall

```sh
sudo ufw allow 5432/tcp
sudo ufw reload
```


### 3. Nvm installation

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
# or
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

```sh
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
```

```sh
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

```sh
source ~/.bashrc   # ou source ~/.zshrc, ou source ~/.profile
```

>
> After finishing, restart the terminal to load the new information.
>

#### 3.1 Nodejs installation

* Installing Node.js using NVM, a version manager that allows us to switch between different versions of Node.js easily.

```sh
nvm install 20
```

### 4. pm2 installation
```sh
npm i -g pm2
```

### 5. Application startup

Cloning the Repository
```
git clone https://github.com/natanael-melo/api-whatsapp-pro.git
```

Go to the project directory and install all dependencies.

>
> Give preference to **npm** as it has greater compatibility.
>

```sh
cd api-whatsapp-pro

npm install
# or
npm install --force
```

### 6. Environment variables
See additional settings that can be applied through the **env** file by clicking **[here](./.env.dev)**.

> **⚠️Attention⚠️:** copy the **.env.dev** file to **.env**.
```sh
cp .env.dev .env
```

### 7. Prism ORM

* We're going to use Prisma ORM to manage our database. Prisma simplifies database access and ensures operations are secure and easy to maintain.
* **Commands and Explanations:**
  * **In development environment: npx prisma migrate dev**
    * We use `migrate dev` in development to automatically create and apply migrations, making working with the database easier.
  * **In production environment: npx prisma migrate deploy**
    * In production, we use `migrate deploy` to apply migrations in a controlled and secure way.
  * **Data visualization:** `npx prisma studio`
    * Prisma Studio is a visual tool that helps us manage and visualize bank data in an intuitive way.

Define the DATABASE_URL the environment variable for the database deployment.

* Performing the database [deployment](https://www.prisma.io/docs/orm/reference/prisma-cli-reference#migrate-deploy).
```sh
bash deploy_db.sh
```

Finally, run the command below to start the application:
```sh
npm run start:dev
# or
npm run start:prod

# pm2
pm2 start 'npm run start:prod' --name CodeChat_API_v1.3.4
# or
pm2 start 'npm run start:dev' --name CodeChat_API_v1.3.4
```
---

## Worker

### Worker options for session management

- **[session-manager:files-v0.0.1](https://github.com/code-chat-br/session-manager)**
- **[session-manager:sqlite-v0.0.1](https://github.com/code-chat-br/session-manager/tree/sqlite)**

To use the worker with the API it is necessary to define the following environment variables in the API:

- `PROVIDER_ENABLED=true`: This variable enables the use of the provider (worker) in the API.
- `PROVIDER_HOST=127.0.0.1`: Defines the host where the worker is listening for requests.
- `PROVIDER_PORT=5656`: Defines the port where the worker is listening for requests.
- `PROVIDER_PREFIX=codechat`: Set prefix for instance grouping on worker

---

## WebSocket
websocket compatibility added.
[Read here.](./src/websocket/Readme.md)


## Authentication

You can define two authentication **types** for the routes in the **[env file](./env.dev)**.
Authentications must be inserted in the request header.

1. **jwt:** A JWT is a standard for authentication and information exchange defined with a signature.

> Authentications are generated at instance creation time.

**Note:** There is also the possibility to define a global api key, which can access and control all instances.


## Send Messages
|     |   |
|-----|---|
| Send Text | ✔ |
| Send Media: audio - video - image - document - gif | ✔ |
| Send Media File | ✔ |
| Send Audio type WhatsApp | ✔ |
| Send Audio type WhatsApp - File | ✔ |
| Send Location | ✔ |
| Send Link Preview | ✔ |
| Send Contact | ✔ |
| Send Reaction - emoji | ✔ |


## Webhook Events

| Name | Event | TypeData | Description |
|------|-------|-----------|------------|
| QRCODE_UPDATED | qrcode.updated | json | Sends the base64 of the qrcode for reading |
| CONNECTION_UPDATE | connection.update | json | Informs the status of the connection with whatsapp |
| MESSAGES_SET | message.set | json | Sends a list of all your messages uploaded on whatsapp</br>This event occurs only once |
| MESSAGES_UPSERT | message.upsert | json |  Notifies you when a message is received |
| MESSAGES_UPDATE | message.update | json | Tells you when a message is updated |
| SEND_MESSAGE | send.message | json | Notifies when a message is sent |
| CONTACTS_SET | contacts.set | json | Performs initial loading of all contacts</br>This event occurs only once |
| CONTACTS_UPSERT | contacts.upsert | json | Reloads all contacts with additional information</br>This event occurs only once |
| CONTACTS_UPDATE | contacts.update | json | Informs you when the chat is updated |
| PRESENCE_UPDATE | presence.update | json |  Informs if the user is online, if he is performing some action like writing or recording and his last seen</br>'unavailable' | 'available' | 'composing' | 'recording' | 'paused' |
| CHATS_SET | chats.set | json | Send a list of all loaded chats |
| CHATS_UPDATE | chats.update | json | Informs you when the chat is updated |
| CHATS_UPSERT | chats.upsert | json | Sends any new chat information |
| GROUPS_UPSERT | groups.upsert | JSON | Notifies when a group is created |
| GROUPS_UPDATE | groups.update | JSON | Notifies when a group has its information updated |
| GROUP_PARTICIPANTS_UPDATE | group-participants.update | JSON | Notifies when an action occurs involving a participant</br>'add' | 'remove' | 'promote' | 'demote' |
| NEW_TOKEN | new.jwt | JSON | Notifies when the token (jwt) is updated


## SSL

To install the SSL certificate, follow the **[instructions](https://certbot.eff.org/instructions?ws=other&os=ubuntufocal)** below.

# Note

This code is in no way affiliated with WhatsApp. Use at your own discretion. Don't spam this.

This code was produced based on the baileys library and it is still under development.
