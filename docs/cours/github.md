# GitHub CI

<center><img src="https://louisonsarlinmagnus.github.io/TurboNAS/img/logo_github.png" width="350"/></td></center>


!!! summary "Définition"
    C'est un service web d'hébergement et de gestion de développement de logiciels  
    *Wikipédia*

    Les Github Actions vont nous permettre d'automatiser l'intégration de notre code.

## Construction du workflow

!!! example
    ```yaml
    name: .NET

    on:
    push:
        branches: [ master ]

    jobs:
    build:
        runs-on: ubuntu-latest

        steps:
        - uses: actions/checkout@v2

        - name: Setup .NET
        uses: actions/setup-dotnet@v1
        with:
            dotnet-version: 3.1.x
    
        - name: Dotnet Publish
        run: dotnet publish -c Release -r linux-x64 -p:PublishSingleFile=true

        - name: Login
        uses: docker/login-action@v1
        with:
            username: ${{ secrets.DOCKER_USERNAME }}
            password: ${{ secrets.DOCKER_PASSWORD }}
        - name: Build&Push
        uses: docker/build-push-action@v2.3.0
        with:
            tags: louisonsarlinmagnus/ezdvf:latest
            push: true

    ```