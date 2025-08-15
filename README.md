# Script de Análise de Sentimento com AWS Comprehend
<p align="center">
  <img src="Amazon Comprehend.png" alt="Análise de Sentimento com AWS Comprehend" width="200"/>
</p>

Este script utiliza o Amazon Comprehend para analisar sentimentos em textos em português.

## Pré-requisitos

- Python 3.x
- Biblioteca boto3: `pip install boto3`
- Credenciais AWS configuradas
- Usuário IAM com permissões adequadas

## Configuração de Permissões IAM

### Política IAM Necessária

Para usar o AWS Comprehend, você precisa criar um usuário IAM com as seguintes permissões:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "comprehend:DetectSentiment",
                "comprehend:DetectLanguage",
                "comprehend:DetectEntities",
                "comprehend:DetectKeyPhrases",
                "comprehend:DetectSyntax"
            ],
            "Resource": "*"
        }
    ]
}
```

### Como criar o usuário IAM:

1. **Acesse o Console AWS** → IAM
2. **Usuários** → **Criar usuário**
3. **Nome do usuário:** `comprehend-user`
4. **Marque:** "Chave de acesso programático"
5. **Próximo: Permissões**
6. **Anexar políticas existentes diretamente**
7. **Criar política** → Cole o JSON acima
8. **Salve as credenciais** (Access Key ID e Secret Access Key)

## Como funciona

### 1. Importação da biblioteca
```python
import boto3
```
- Importa o SDK da AWS para Python
- Permite conectar e usar serviços AWS

### 2. Configuração do cliente Comprehend
```python
comprehend = boto3.client(
    'comprehend',
    region_name='us-east-2',
    aws_access_key_id='AOPLTZT52YLQ7MBA4KIA',
    aws_secret_access_key='CEhdKFK2q5gVm3wy1g2LiJrtz6Jc4TebZiQNjj5'
)
```
- Cria conexão com o serviço Amazon Comprehend
- `region_name`: Define região AWS (Ohio)
- `aws_access_key_id`: Sua chave de acesso
- `aws_secret_access_key`: Sua chave secreta
- Autentica você na AWS

### 3. Definição do texto
```python
texto = "Esse curso está incrível!"
```
- Define o texto que será analisado
- Pode ser qualquer frase em português

### 4. Análise de sentimento
```python
resposta = comprehend.detect_sentiment(Text=texto, LanguageCode='pt')
```
- Chama a API do Comprehend
- `Text=texto`: Envia o texto para análise
- `LanguageCode='pt'`: Especifica idioma português
- Retorna análise completa do sentimento

### 5. Exibição dos resultados
```python
print("Sentimento detectado:", resposta['Sentiment'])
print("Scores:", resposta['SentimentScore'])
```
- `resposta['Sentiment']`: Mostra POSITIVE, NEGATIVE, NEUTRAL ou MIXED
- `resposta['SentimentScore']`: Mostra probabilidades (0-1) para cada sentimento

## Resultados possíveis

- **POSITIVE**: Sentimento positivo
- **NEGATIVE**: Sentimento negativo  
- **NEUTRAL**: Sentimento neutro
- **MIXED**: Sentimento misto

## Código Completo
```python
"""
Usando 
"""
import boto3

# Configure suas credenciais IAM aqui
comprehend = boto3.client(
    'comprehend',
    region_name='us-east-2',
    aws_access_key_id='AKIAXZTYUJOQ7MBA4KIA',
    aws_secret_access_key='CEhdKFK2q5gVm3wy1g2HIilTERfWz6Jc4TebZiQNjj5'
)

texto = "Esse curso está incrível!"
resposta = comprehend.detect_sentiment(Text=texto, LanguageCode='pt')

print("Sentimento detectado:", resposta['Sentiment'])
print("Scores:", resposta['SentimentScore'])


"""
outro tradutor
"""
import boto3

translate = boto3.client(
    'translate',
    region_name='us-east-2',
    aws_access_key_id='AKIAXZT5PLUN7MBA4KIA',
    aws_secret_access_key='CEhdKFK2q5gVm3wy1g2HTpPTHJWz6Jc4TebZiQNjj5'
)

resposta = translate.translate_text(
    Text='Hello, how are you?',
    SourceLanguageCode='en',
    TargetLanguageCode='pt'
)

print(resposta['TranslatedText'])
```

## Como executar

```bash
python "Script tradutor.py"
```

## Segurança

⚠️ **Importante**: Nunca compartilhe suas credenciais AWS!

### Alternativa mais segura - Variáveis de ambiente:

```python
import os
import boto3

comprehend = boto3.client(
    'comprehend',
    region_name='us-east-2',
    aws_access_key_id=os.getenv('AWS_ACCESS_KEY_ID'),
    aws_secret_access_key=os.getenv('AWS_SECRET_ACCESS_KEY')
)
```

Defina as variáveis no PowerShell:
```bash
$env:AWS_ACCESS_KEY_ID="sua_access_key"
$env:AWS_SECRET_ACCESS_KEY="sua_secret_key"
```

## Observações

O Amazon Comprehend analisa o texto e determina se é positivo, negativo, neutro ou misto com scores de confiança para cada categoria. O script também inclui funcionalidade de tradução usando o AWS Translate.
