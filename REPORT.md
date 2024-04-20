# REPORT

## 2. Dockerizes a Node.js application.

Tại thư mục `src/`, tạo một tệp `Dockerfile` với nội dung như sau:

```
FROM node:10-alpine.

RUN mkdir -p /home/honeypot/intern_app

RUN mkdir -p /home/honeypot/intern_app/node_modules

WORKDIR /home/honeypot/intern_app

COPY package*.json ./

RUN adduser -D honeypot

RUN chown -R honeypot:honeypot /home/honeypot/intern_app

RUN chmod -R 755 /home/honeypot/intern_app/node_modules

USER honeypot

RUN npm install

COPY --chown=honeypot:honeypot . .

EXPOSE 3000

CMD [ "node", "index.js" ]
```

Xây dựng docker image thực hiện lệnh `sudo docker build -t honeypot/demo .`

Chạy container vừa được xây dựng `sudo docker run --name demo -p 3000:3000 -d honeypot/demo`

## 3. Establishes an automated CI/CD build process using GitHub Actions workflow and a container registry service such as DockerHub or Amazon Elastic Container Registry (ECR) or similar services

Tạo một folder là `.github`, tiếp theo tạo một folder khác có tên là `workflows` bên trong folder `.github`.

Tiếp theo tạo một tệp có tên là `build.yaml` bên trong thư mục `workflows`

```
name: intern-devops

on:
  push:
    branch: ["master"]

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      
      - name: Login Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_ACCESS_TOKEN }}
      
      - name: Build and Push Docker Image
        uses: docker/build-push-action@v4
        with:
          context: ./src
          file: .Dockerfile
          push: true
          tag: ${{ secrets.DOCKERHUB_USERNAME }}/intern-devops:latest
```

Docker repo: <https://hub.docker.com/r/2052120603/intern-devops>

## 4. Initiates CI tests automatically when changes are pushed to the feature branch on GitHub

Tạo một tệp tên là `test-feature-branch.yaml` trong thư mục `.github/workflows`:

```
name: devops-intern-test_feature_branch

on:
  push:
    branches-ignore:
      - master

jobs:
  feature-brand-tests:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        with:
          version: ${{ matrix.node-version }}
          cache: 'npm'
      - name: Install dependencies
        run: npm ci
        working-directory: src/
      - name: NPM Build
        run: npm run build --if-present
        working-directory: src/
      - name: Test
        run: npm test
        working-directory: src/
```
