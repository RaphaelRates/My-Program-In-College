> [!note]
> ### Métodos de detecção de Faces
> Existem diversas formas de fazer um único código. Neste usaremos 3 métodos:
> - Haarcascade
> - CNN e Dlib
> - HOG e Dlib

> [!warning] Normalmente podemos pegar as imagens e jogá-las no algoritmo [[AdaBoost]], mas podemos fazer de mais de uma forma.

> [!tip]- # Método Haarcascade
> ## Code
> ### Etapa 1
> Primeiro importamos as bibliotecas para executar o código:
> ```python
> import cv2
> 
> # Caso você use o Google Colab
> from google.colab import drive
> drive.mount('/content/drive')
> ```
> 
> ### Etapa 2
> Pegamos a imagem ( ͡° ͜ʖ ͡°)
> ```python
> image = cv2.imread("caminho da imagem")
> 
> # Se você estiver no Colab, provavelmente terá a opção de pegar a imagem por meio do Google Drive
> image = cv2.imread("/content/drive/MyDrive/caminho/da/imagem.jpg")
> ```
> 
> ### Etapa 3 *OPCIONAL*
> Redimensionar a Imagem e depois colocá-la em [[Escala de Cinza]]:
> ```python
> image = cv2.resize(image, (600, 800))
> image.shape  # (600, 800, 3) se for colorida
> 
> image_gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
> # cv2.imshow('Imagem em escala de cinza', image_gray)  # caso queira mostrar a imagem
> # cv2.waitKey(0)
> # cv2.destroyAllWindows()
> ```
> 
> ### Etapa 4
> Definir o Classificador Cascade:
> ```python
> detector_faces = cv2.CascadeClassifier("caminho/do/classificador/cascade.xml")
> deteccoes_localizadas = detector_faces.detectMultiScale(image_gray)
> ```
> 
> Aqui vale uma explicação mais detalhada sobre o que está acontecendo:
> - Esse comando **carrega um arquivo XML** que contém um **modelo treinado com Haar Cascades** (ou LBP, dependendo do XML).
> - O arquivo tem uma estrutura com **vários estágios** de classificadores fracos (em geral, _árvores de decisão rasas_).
> - Cada estágio é treinado para identificar **características simples** (como bordas, gradientes ou sombras) que compõem um rosto.
> 
> - O classificador trabalha em cascata:
> 	- As janelas que falham em estágios iniciais são **descartadas rapidamente**.
> 	- Somente as regiões "promissoras" passam para os estágios seguintes.
> - Essa função **varre a imagem em múltiplas escalas (tamanhos)** e posições.
> 
> - Para cada subjanela da imagem:
> 	1. Extrai as **features Haar/LBP** correspondentes.
> 	2. Passa pelos estágios da cascata.
> 	3. Se sobreviver a todos → é considerada **detecção válida**.
> 
> - O "MultiScale" significa que ele **redimensiona a imagem repetidamente**, buscando rostos grandes e pequenos.
> 
> ###🧩 **Etapa 5 — Exibição das detecções**
>
> Depois que as coordenadas das faces são detectadas, precisamos **marcá-las visualmente** na imagem para verificar o resultado.
>
> ```python
> # Percorre todas as detecções retornadas pelo classificador 
> for (x, y, w, h) in deteccoes_faces:     
> 	# Desenha um retângulo amarelo (BGR → 0,255,255)     
> 	# (x, y) = canto superior esquerdo     
> 	# (x + w, y + h) = canto inferior direito     
> 	# 5 = espessura da borda     
> 	cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 255), 5)
> 
> # Exibe a imagem resultante em uma janela 
> cv2.imshow("Detecção de Faces", image) 
> cv2.waitKey(0)     # aguarda uma tecla para fechar cv2.destroyAllWindows()
> ```
> 
> ### ⚙️ **O que acontece por baixo dos panos**
> 1. **Loop de detecções**
> - `deteccoes_faces` é uma lista de tuplas `(x, y, w, h)` produzida pelo `detectMultiScale`.
> - Cada tupla representa uma **janela delimitadora (bounding box)** de um rosto.
> 
> 2. **Desenho com `cv2.rectangle()`**
> - A função desenha direto sobre o array da imagem (que é uma matriz NumPy).
> - OpenCV usa o formato **BGR** (não RGB), por isso `(0,255,255)` resulta em **amarelo**.
> - Isso **não cria uma nova imagem** — ele altera o buffer atual.


> [!info] ### **Como o Haar Cascade funciona internamente**
>
> 2. **Pré-processamento:** a imagem é convertida para tons de cinza.
> 3. **Cálculo da Imagem Integral:** soma cumulativa dos pixels → agiliza cálculo das features.
> 4. **Features Haar:** compara regiões claras e escuras da janela para identificar padrões (ex: olhos mais escuros que bochechas).
> 5. **AdaBoost:** combina vários classificadores simples em um forte.
> 6. **Cascata:** aplica os classificadores em série → elimina janelas negativas cedo.
> 
> 📊 Esse processo torna a detecção **muito rápida**, ideal para uso em tempo real (como em webcams).

> [!cite] 🧩 Em resumo:  
> O `CascadeClassifier` é um pipeline de **filtros sequenciais treinados supervisionadamente** usando o método **AdaBoost**.

> [!tip]- # Método HOG + Dlib
> ## Code
> ### Etapa 1
> Primeiro importamos as bibliotecas para executar o código:
> ```python
> import cv2
> 
> # Caso você use o Google Colab
> from google.colab import drive
> drive.mount('/content/drive')
> ```
> 
Show — bora falar do **HOG (Histogram of Oriented Gradients)**, sem floreio, mas com cérebro.  
E já te deixo tudo **em formato Obsidian**, pronto pra colar no vault como nota técnica.
>
> ### ⚙️ **Ideia principal**
>
> “Se eu entender **pra onde a imagem muda** (os gradientes), eu entendo o formato do objeto.”
>
> O HOG **divide a imagem em pequenas células**, calcula **para onde as bordas estão apontando** (orientações), e depois cria um **histograma dessas direções**.  Isso gera uma **assinatura numérica** do formato do objeto — perfeita pra usar com classificadores como [[ML - SVM Maquina Vetorial de Suporte]].
>
> ## 🧩 **Etapas do Algoritmo**
>
> ### **1️⃣ Pré-processamento**
>
> - Converte a imagem pra **tons de cinza** → simplifica os cálculos.
>    
> - Normaliza o contraste (às vezes usa equalização de histograma).
>     
> 
> ```python
> img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
> ```
> 
> 
> ### **2️⃣ Cálculo dos Gradientes**
> 
> - Calcula o quanto a intensidade muda em **x** e **y**:
 >    
> 
> ```python
> gx = cv2.Sobel(img, cv2.CV_32F, 1, 0, ksize=1)
> gy = cv2.Sobel(img, cv2.CV_32F, 0, 1, ksize=1)
> magnitude, angle = cv2.cartToPolar(gx, gy, angleInDegrees=True)
> ```
>
> 🧮
>
> - **Magnitude:** quão forte é a borda.
   >  
> - **Ângulo:** direção da borda (0°–180°).
>   
>
> ### **3️⃣ Divisão em células**
>
> - A imagem é cortada em **pequenos blocos** (ex: 8×8 pixels).
  >  
> - Em cada célula, cria-se um **histograma das direções** dos gradientes.
   >  
> 
> Exemplo:
> 
| Faixa de ângulo | Contagem |
| --------------- | -------- |
| 0–20°           | 12       |
| 20–40°          | 5        |
| 40–60°          | 3        |
| …               | …        |
>
> Assim, cada célula vira um **vetor de 9 valores** representando as orientações dominantes.
> 
> ### **4️⃣ Normalização em blocos**
> 
> - Junta células vizinhas (ex: 2×2) pra formar **blocos**.
> - Normaliza os vetores — isso corrige variações de luz e contraste.
> 👉 Isso é o segredo que torna o HOG robusto.

---

### **5️⃣ Vetor final**

- Todos os blocos são concatenados num **grande vetor de características**.
    
- Esse vetor pode ter **milhares de dimensões**, mas representa a **“forma”** do objeto.
    

---

### **6️⃣ Classificação**

- Esse vetor é passado pra um classificador (geralmente um **SVM linear**).
    
- O SVM aprende quais padrões de gradiente representam, por exemplo, **um rosto ou um pedestre**.
    

---

## ⚙️ **Resumo do pipeline**

```mermaid
graph TD
A[Imagem de entrada] --> B[Conversão para tons de cinza]
B --> C[Cálculo dos gradientes]
C --> D[Divisão em células 8x8]
D --> E[Histograma de orientações]
E --> F[Normalização em blocos]
F --> G[Vetor final de características]
G --> H[Classificador (ex: SVM)]
```

---

## 📊 **Características do HOG**

|Vantagem|Descrição|
|---|---|
|Rápido|Pode ser usado em tempo real|
|Interpretável|Baseado em bordas e formas, não em redes neurais|
|Robusto à iluminação|Graças à normalização por blocos|
|Limitado|Não reconhece bem objetos muito deformáveis ou complexos|

---

### 💡 **Exemplo prático (OpenCV + HOGDescriptor)**

```python
hog = cv2.HOGDescriptor()
hog.setSVMDetector(cv2.HOGDescriptor_getDefaultPeopleDetector())

image = cv2.imread("pessoas.jpg")
(rects, weights) = hog.detectMultiScale(image, winStride=(4,4), padding=(8,8), scale=1.05)

for (x, y, w, h) in rects:
    cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)
```

---

## 🧩 **Resumo técnico**

|Etapa|O que faz|Resultado|
|---|---|---|
|Gradiente|Calcula mudanças de intensidade|Direção e força|
|Células|Agrupa gradientes locais|Histograma de 9 bins|
|Blocos|Normaliza e concatena células|Vetor robusto|
|Classificador|Usa SVM para decisão|Detecção final|

---

## 🧠 **Em essência**

> O HOG transforma uma imagem em um mapa de direções de bordas, criando uma “assinatura geométrica” do objeto.  
> É o cérebro dos detectores clássicos de **pedestres e rostos** antes da era das CNNs.

---

Quer que eu te monte uma **nota complementar** com comparação HOG vs CNN (visão clássica vs profunda)? Dá pra entender bem a evolução da área.