# Lab Kubernetes

Objetivo do lab é cria um cluster de Kubernetes no OCI com node pool ARM e subir um servidor web rodando uma página web estática.

## Lab

1. [Criação do Cluster](#criação-do-cluster)
2. [Acessando o Cluster via Cloud Shell](#acessando-o-cluster-via-cloud-shell)
3. [Realizando o Deploy da Aplicação](#realizando-o-deploy-da-aplicação)
4. [Acessando a aplicação](#acessando-a-aplicação)

## Criação do Cluster

Nave gue no menu da Oracle Cloud e vá em Developer Services -> Kubernetes Cluster (OKE).

![consoleoci](images/consoleoci.png)

Clique em **Create Cluster**, aparecerá um popup onde será selecion a opção **Quick Create** e clique em **Create**.

![okeconsole](images/okeconsole1.png)

![okeconsole](images/okeconsole2.png)

Na tela de configuração vamos preencher os campos da seguinte maneira:

- **Kubernetes Version**: v1.24.1
- **Kubernetes API Endpoint**: Public Endpoint
- **Kubernetes worker nodes**: Private workers
- **Shape**: VM.Standard.A1.Flex
- **Select the number of OCPUs**: 2
- **Amount of Memory (GB)**: 16
- **Number of nodes**: 2

![okeconsole](images/okeconsole3.png)

Após preencher os campos basta clicar em Next e verá uma tela como a da imagem abaixo. Onde basta clicar em **Create Cluster**.

![okeconsole](images/okeconsole4.png)

Espere alguns minutos para a conclusão da criação completa do cluster.

## Acessando o Cluster via Cloud Shell

Navegue novamente pelo menu do OCI Developer Services -> kubernetes Cluster (OKE). Clique no cluster criado no passo anterior.

![oke](images/oke1.png)

Na página de detalhes do cluster clique em **Access Cluster**, um popup irá aparecer na tela. Copie o código que aparece no passo 2 do popup e depois clique no botão **Launch Cloud Shell**.

![oke](images/oke2.png)

![oke](images/oke3.png)

Com o Cloud Shell aberto, cole e execute o código copiado no passo anterior. Após a execução do código valide se o acesso foi criado corretamente executando o código abaixo.

``` 
$ kubectl get nodes
```
Resultado
``` 
NAME          STATUS   ROLES   AGE    VERSION
10.0.10.202   Ready    node    126m   v1.24.1
10.0.10.232   Ready    node    142m   v1.24.1
```

## Realizando o Deploy da Aplicação

Faça o download do manifesto kubernetes executando o código abaixo no cloudshell.

```
wget https://github.com/ChristoPedro/lab-oke/releases/latest/download/deploy.yaml
```

Após o download execute o seguinte comando para subir a aplicação para o OKE criado nos passos anteres.

```
kubectl deploy -f deploy.yaml
```
Resultado
```
deployment.apps/my-nginx created
service/my-nginx-svc created
```

## Acessando a aplicação

Execute o seguinte comando:

```
kubectl get svc my-nginx-svc
```

Resultado

```
NAME           TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
my-nginx-svc   LoadBalancer   10.96.28.117   132.226.63.163   80:30578/TCP   66s
```

Copie o EXTERNAL-IP e cole no navegador.




