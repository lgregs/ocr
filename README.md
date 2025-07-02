# Extrator de Texto de PDFs com OCR e Visualização

Este projeto é uma ferramenta desenvolvida em Python para extrair texto de documentos PDF que não funcionam com extratores comuns, ou o PDF é uma imagem ou esta quebrado. A solução utiliza uma abordagem de duas etapas: primeiro, converte cada página do PDF em uma imagem, em seguida, aplica técnicas de Reconhecimento Óptico de Caracteres (OCR) para extrair o texto de cada imagem.

Um diferencial deste projeto é a capacidade de gerar imagens anotadas, onde cada bloco de texto reconhecido é destacado com uma "bounding box" (caixa delimitadora), permitindo uma verificação visual da precisão do processo de OCR.

## Funcionalidades

  * **Conversão de PDF para Imagem**: Converte arquivos PDF de várias páginas em imagens PNG individuais de alta qualidade (300 DPI).
  * **Extração de Texto com OCR**: Utiliza a biblioteca `EasyOCR` para identificar e extrair texto de imagens em inglês.
  * **Filtragem por Confiança**: Apenas o texto com um score de confiança acima de um limiar pré-definido (neste caso, 25%) é considerado, aumentando a precisão do resultado final.
  * **Visualização com Bounding Box**: Gera cópias das imagens originais com o texto detectado e as caixas delimitadoras desenhadas sobre elas, facilitando a validação dos resultados.
  * **Organização de Arquivos**: Salva as imagens processadas e anotadas em um diretório separado (`processed_images`) para manter o projeto organizado.

## Como Funciona a Pipeline

O processo é executado em duas fases principais:

1.  **Conversão do Documento (`PyMuPDF`)**: A biblioteca `PyMuPDF` é utilizada para abrir o arquivo PDF e renderizar cada página como uma imagem PNG com `300 DPI`. Essa alta resolução é crucial para melhorar a qualidade e a precisão do OCR na etapa seguinte.

2.  **Reconhecimento e Anotação (`EasyOCR` e `OpenCV`)**:

      * Para cada imagem gerada, a biblioteca `EasyOCR` é acionada para realizar o OCR.
      * Os resultados são uma lista de tuplas, cada uma contendo a caixa delimitadora, o texto detectado e a pontuação de confiança.
      * O texto é filtrado com base no limiar de confiança.
      * A biblioteca `OpenCV` é usada para desenhar as caixas delimitadoras verdes e o texto correspondente em azul sobre as imagens, que são salvas no diretório `processed_images`.
      * O texto extraído de todas as páginas é concatenado e exibido no console ao final da execução.

## Tecnologias Utilizadas

  * **PyMuPDF (`fitz`)**: Para manipulação e conversão de arquivos PDF.
  * **EasyOCR**: Um motor de OCR poderoso e de fácil utilização.
  * **OpenCV-Python (`cv2`)**: Para manipulação de imagens e desenho das caixas delimitadoras.
  * **os**: Para manipulação de diretórios e caminhos de arquivos.

## Instalação e Configuração

1.  **Clone este repositório:**

    ```bash
    git clone https://github.com/lgregs/ocr
    ```

2.  **Crie um ambiente virtual (Altamente Recomendado):**
- Essas libs geralmente possuem muitas dependencias, então é quase certeza que vai quebrar algum outro projeto seu se ela trouxer alguma lib com uma versão diferente.
  
    ```bash
    python -m venv venv
    source venv/bin/activate  # No Windows: venv\Scripts\activate
    ```

##  Como Executar
- Vai depender se você está usando um notebook python como eu ou se vai executar só no python mesmo.

1.  Abra o arquivo de script principal (`easyOCR.ipynb`).
2.  Altere a variável `pdf_path` para o caminho completo do arquivo PDF que você deseja processar:
    ```python
    # Inserir caminho completo do PDF
    pdf_path = 'C:/caminho/para/seu/documento.pdf'
    ```
3. Execute todas as células do caderno.
   ou
  Execute o script a partir do seu terminal:
    ```bash
    python easyOCR.ipynb
    ```

## Resultados Esperados

Após a execução do script:

1.  **Saída no Console**: O terminal exibirá o progresso da conversão e da extração, e ao final, imprimirá todo o texto extraído do documento PDF.
2.  **Arquivos Gerados**:
      * Na mesma pasta do PDF original, serão criadas imagens PNG para cada página (ex: `paginaTeste_1.png`).
      * Um novo diretório chamado `processed_images` será criado. Dentro dele, você encontrará as mesmas imagens, mas com as anotações das caixas delimitadoras e o texto detectado.

## Possíveis Melhorias Futuras

  * **Interface de Linha de Comando (CLI)**: Implementar `argparse` para permitir que o usuário passe o caminho do PDF como um argumento de linha de comando, em vez de alterar o código.
  * **Suporte a Múltiplos Idiomas**: Modificar a inicialização do `EasyOCR` (`easyocr.Reader(['en', 'pt'])`) para suportar outros idiomas, como o português.
  * **Estruturação da Saída**: Em vez de imprimir texto puro, salvar a saída em um formato estruturado como JSON, contendo o texto, a página e as coordenadas da caixa delimitadora.
  * **Interface Gráfica**: Desenvolver uma interface simples com `Streamlit` ou `Flask` para que usuários possam fazer o upload de arquivos PDF e visualizar os resultados de forma interativa.
  * **Tratamento de Erros**: Adicionar blocos `try-except` para lidar com arquivos PDF corrompidos ou não encontrados.
