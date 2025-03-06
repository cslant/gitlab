# CSlant gitlab project

This project is a simple project to set up a gitlab server with docker-compose.

## How to use

1. Clone the repository

```bash
git clone https://gitlab.com/cslant/gitlab.git
```

2. Change to the repository directory

```bash
cd gitlab
```

3. Create the environment file

```bash
cp .env.example .env
```

4. Edit the environment file

```bash
nano .env
```

5. Start the gitlab server

```bash
docker-compose up -d
```

6. Access the gitlab server

Open your browser and go to `http://localhost:802`
