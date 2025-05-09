# Processamento de Linguagem Natural

# Equipe

* Cristian Prochnow
* Gustavo Henrique Dias
* Lucas Willian de Souza Serpa
* Marlon de Souza
* Ryan Gabriel Mazzei Bromati

# Sobre

O principal objetivo desse projeto é ser um identificador de emoções, ao qual, como se fosse em um vídeo do YouTube, é possível inserir comentários distintos sobre o conteúdo e então o modelo reconhece qual o sentimento possivelmente relacionado com esse comentário.

## Treinamento

O treinamento do modelo foi feito a partir de um dataset de comentários, com pouco mais de 3000 linhas, que possui então duas colunas: comentário e emoção relacionada. Esse treinamento é feito no momento de inicialização do serviço, então todas as requisições feitas usam o mesmo modelo como base, a partir de quando o server foi iniciado.

### Exemplo do dataset

```csv
comment,emotion
"Amei demais esse vídeo, me deixou muito feliz!",alegria
"Que notícia triste, fiquei bem chateado agora.",tristeza
"Nossa, que susto! Não esperava por essa.",surpresa
...
```

Acesse o dataset [aqui](https://github.com/SpotifaiI/natural-language-emotioner/blob/main/comments.csv).

## Repositórios

* [IA](https://github.com/SpotifaiI/natural-language-emotioner)
* [Chat](https://github.com/SpotifaiI/natural-language-asker)

# Prática

Fizemos então como se fosse um chat, ao qual há um [serviço rodando](https://github.com/SpotifaiI/natural-language-emotioner) com o modelo pronto para reconhecimento, e paralelo a isso uma [interface web](https://github.com/SpotifaiI/natural-language-asker) que serve como chat para o processamento.

## Processamento

Para rodar o projeto, basta executar os comandos abaixo.

```shell
# usando Docker
$ docker compose up



# ou se quiser rodar os pacotes manualmente
$ pip install -r requirements.txt
$ fastapi run app.py
```

As rotas para uso estão todas no arquivo [de requisições](./requests.http).

A API é formada por duas rotas principais (além do índice), sendo então a rota de `/ask`, ao qual é necessário enviar um JSON no formato abaixo.

```json
{
  "comment": "conteúdo bom demais"
}
```

Recebendo então, como resposta, o sentimento representado por aquele comentário que foi enviado.

```json
{
  "success": true,
  "emotion": "alegria",
  "comment": "conteúdo bom demais"
}
```

E, para análise, há a rota `/stats`, que fica responsável por realizar todos os cálculos referentes às métricas de algoritmos de classificação, para avaliarmos melhor o processamento Nayve Bayes.

```json
{
  "success": true,
  "metrics": {
    "accuracy": 1.0,
    "precision": 1.0,
    "recall": 1.0,
    "f1_score": 1.0,
    "confusion_matrix": [
      [
        1,
        0
      ],
      [
        0,
        2
      ]
    ],
    "classification_report": {
      "alegria": {
        "precision": 1.0,
        "recall": 1.0,
        "f1-score": 1.0,
        "support": 1.0
      },
      "desgosto": {
        "precision": 1.0,
        "recall": 1.0,
        "f1-score": 1.0,
        "support": 2.0
      },
      "accuracy": 1.0,
      "macro avg": {
        "precision": 1.0,
        "recall": 1.0,
        "f1-score": 1.0,
        "support": 3.0
      },
      "weighted avg": {
        "precision": 1.0,
        "recall": 1.0,
        "f1-score": 1.0,
        "support": 3.0
      }
    }
  }
}
```

Exemplos podem ser encontrados diretamente no arquivo de requisições citado anteriormente.

## Chat

E aqui no chat, temos então uma interface que simula um visualizador de vídeos, com um vídeo no top, um chat logo após que interage commo serviço de análise e também links de referência para visualização dos dados.

### Vídeo

![Vídeo](https://github.com/user-attachments/assets/60fee470-938e-481d-92bd-0ef65216cbf4)

### Chat

![Chat](https://github.com/user-attachments/assets/a6311205-0372-444b-ab16-cd43f759b7f6)

### Links

![image](https://github.com/user-attachments/assets/1e41620b-0080-4b71-9581-5e9b95b6eb02)

Nessa tela de links temos ao todo 4 links, sendo eles:

* `Análise de dados` é responsável por baixar um arquivo `.json` que fica responsável por passar os dados de análise das mensagens que foram enviadas até então Vs o conteúdo de treinamento da base original do projeto.
* `Análise íntegra dos dados` é responsável por baixar um arquivo `.json` completo com a análise feita da base de dados original Vs pedaço de treinamento aleatório da base original, determinado pelo processo.
* `Repositório IA` é o link do repositório que guarda o código do serviço de análise dos dados com a IA
* `Repositório Web` é o link do repositório que guarda o código do site com o Chat
