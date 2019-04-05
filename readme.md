# Criar entradas no route 53 via Terraform
### Pré-requisitos
- aws cli instalado ```pip install awscli --upgrade --user``` 
- aws cli configurado ```aws configure```
- terraform baiado - https://www.terraform.io/downloads.html
- ID da ZONA a ser alterada
```
aws route53 list-hosted-zones-by-name |  jq --arg name "DOMINIO." -r '.HostedZones | .[] | select(.Name=="\($name)") | .Id'
```

### Alterações necessárias

Colocar as entradas para serem criadas pelo script dentro de um arquivo .CSV conforme exemplo abaixo:

```
entrada-1,domonio.com,192.168.1.1,A,ABC123456
entrada-2,domonio.com,192.168.1.2,A,ABC123456
entrada-3,domonio.com,entrada-2.dominio.com,CNAME,ABC123456
```
onde:

`ENTRADA.DOMINIO,ENDERECO_IP,TIPO_DE_ENTRADA,ID_DA_ZONA
`
Para entradas com mais de um IP utilize a seguinte sintexe no arquivo

`entrada-X,domonio.com,192.168.1.2\"\, "192.168.1.3,A,ABC123456
`

### Execução
#### gera-route53.sh

Ao executar o `./gera-route53.sh` será criado um arquivo `entradas-route53-DOMINIO.tf`. 

#### terraform

Com as credenciais da AWS devidamente configuradas pelo `aws configure`, basta rodar os seguintes comandos

##### Criação das entradas

```
terraform init          # irá baixar o provider da AWS para o terraform
terraform plan        # para mostrar quais as entradas serão criadas, apenas para conferencia dos dados
terraform apply      # para aplicar as entradas que estao no entradas.cvs no route 53
```
##### Deleção das entradas

```
terraform destroy          # irá deletar todas as entradas criadas anteriormente pelo passo do apply
```

