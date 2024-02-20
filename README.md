# Test of auto deployment of files into EC2 instances

testing CI-CD with `github actions`.

* testing deploy v1

```
name: Push-to-EC2
on:
  push:
    branches:
      - main
      
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout the files
      uses: actions/checkout@v3

    - name: Install rsync
      run: sudo apt-get update && sudo apt-get install -y rsync

    - name: List files in the current directory
      run: |
        echo "Listando arquivos e diretórios no diretório atual:"
        ls -la

    - name: Sync files with SSH
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.HOST }}
        username: ec2-user
        key: ${{ secrets.EC2_SSH_KEY }}
        port: 22
        script: |
          echo "Iniciando rsync com verbosidade..."
          rsync -avz --delete ./ /home/ec2-user/
          echo "Listando o conteúdo do diretório de destino:"
          ls /home/ec2-user/
```