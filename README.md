# Tensor-Face: Sistema de Detecção e Reconhecimento Facial

![Python](https://img.shields.io/badge/Python-3.11-blue.svg)
![Framework](https://img.shields.io/badge/Framework-TensorFlow-orange.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

Um projeto de Visão Computacional que utiliza o framework TensorFlow para detectar e reconhecer faces em imagens, tanto em arquivos estáticos quanto em tempo real (via webcam).

## Demonstração

O sistema identifica com sucesso múltiplas faces em uma imagem, mesmo com variações de pose e iluminação.

![Demonstração do Tensor-Face](resultado.png?raw=true)
*Exemplo: Reconhecimento dos chefs Gordon Ramsay, Aaron Sanchez e Christina Tosi.*

## Tecnologias Utilizadas

- **Framework Principal:** TensorFlow 2.x
- **Detecção de Face:** MTCNN (Multi-task Cascaded Convolutional Networks)
- **Extração de Features (Embeddings):** FaceNet
- **Processamento de Imagem:** OpenCV
- **Manipulação de Dados:** NumPy, Scikit-learn
- **Visualização:** Matplotlib
- **Ambiente:** Jupyter Notebook

## Estrutura do Projeto

Para executar o projeto, seu diretório deve seguir a estrutura abaixo:

``` bash
tensor-face/
├── database/
│   ├── nome_da_pessoa_1/
│   │   ├── imagem1.jpg
│   │   └── imagem2.png
│   └── nome_da_pessoa_2/
│       └── ...
├── test_images/
│   └── imagem_de_teste.jpg
├── face_recognition.ipynb
├── requirements.txt
└── README.md
```

- `database/`: Armazena as imagens de referência. Crie uma subpasta para cada pessoa que deseja reconhecer.
- `test_images/`: Contém as imagens que você usará para testar o sistema.

## Instalação Local

Siga os passos abaixo para configurar e executar o projeto em seu ambiente local.

### 1. Pré-requisitos

- Git
- Python 3.11

### 2. Clone o Repositório

```bash
git clone https://github.com/ede1000son/tensor-face.git
cd tensor-face
```

### 3. Crie o arquivo `requirements.txt`

Crie um arquivo chamado requirements.txt na raiz do projeto e adicione as seguintes dependências:

```bash
tensorflow
jupyterlab
mtcnn
keras-facenet
opencv-python
matplotlib
scikit-learn
```

### 4. Configure o Ambiente Virtual (Escolha uma opção)

É altamente recomendado usar um ambiente virtual para isolar as dependências do projeto.

#### Opção A: Usando `uv` (Recomendado para velocidade)

```bash
# Crie o ambiente virtual com Python 3.11
uv venv -p python3.11

# Ative o ambiente
source .venv/bin/activate

# Instale os pacotes
uv pip install -r requirements.txt
```

#### Opção B: Usando `conda`

```bash
# Crie o ambiente conda com Python 3.11
conda create -n tensorface python=3.11 -y

# Ative o ambiente
conda activate tensorface

# Instale os pacotes com pip (dentro do ambiente conda)
pip install -r requirements.txt
```

#### Opção C: Usando `pyenv` + `venv`

```bash
# Instale a versão desejada do Python (se ainda não tiver)
pyenv install 3.11.11

# Defina a versão local para a pasta do projeto
pyenv local 3.11.11

# Crie o ambiente venv padrão
python -m venv .venv

# Ative o ambiente
source .venv/bin/activate

# Instale os pacotes
pip install -r requirements.txt
```

## Como Usar

1. **Popule o Banco de Dados**: Adicione imagens de pessoas na pasta `database/`, seguindo a estrutura de pastas.
2. **Inicie o Jupyter Lab**: Com o ambiente virtual ativado, execute:

```bash
jupyter lab
```

3. **Execute o Notebook**: Abra o arquivo `face_recognition.ipynb` e execute as células em ordem. A primeira execução pode demorar um pouco para baixar os modelos pré-treinados.

## Como Funciona

O sistema opera em um fluxo de duas etapas principais:

1. Criação do Banco de Dados de Embeddings:

    - A função `load_face_database` varre a pasta `database`.
    - Para cada imagem, a rede **MTCNN** detecta e recorta o rosto.
    - O rosto recortado é passado para a **FaceNet**, que gera um vetor de 512 dimensões (um *embedding*). Esse *embedding* funciona como uma "impressão digital" matemática única para aquele rosto.
    - O sistema armazena todos os *embeddings* associados aos nomes das pessoas.

2. Processo de Reconhecimento:

    - A função `recognize_faces` recebe uma nova imagem.
    - O **MTCNN** detecta todos os rostos na imagem.
    - Para cada rosto detectado, a **FaceNet** gera um novo *embedding*.
    - Este novo *embedding* é comparado com todos os *embeddings* do banco de dados usando a métrica de **distância cosseno**.
    - O rosto é classificado com o nome correspondente ao *embedding* de menor distância, desde que essa distância esteja abaixo de um limiar de confiança (`threshold`). Caso contrário, é classificado como "Desconhecido".

## Possíveis melhorias

- [ ] Implementar o salvamento e carregamento do banco de dados de embeddings para evitar o reprocessamento a cada execução.
- [ ] Otimizar o código da webcam para processar frames diretamente, sem salvar arquivos temporários.
- [ ] Desenvolver uma interface de usuário simples com Streamlit ou Gradio.
- [ ] Testar com diferentes modelos de detecção e de embedding.

## Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.