# PDI_T1
Relatório Trabalho sobre processamento digital de imagens

1. Introdução
O trabalho teve como objetivo aplicar diversas técnicas de processamento de imagens para analisar e aprimorar a qualidade de imagens médicas, utilizando o conjunto de dados ChestMNIST. O processo envolveu a adição de ruído, equalização de histograma, transformações de intensidade, aplicação de filtros de suavização (Gaussiano, Mediana) e realce de bordas (usando Sobel e Laplaciano). Além disso, foram feitas transformações adicionais, como a correção gama e o uso de técnicas de high-boost para aprimorar as imagens.

2. Fluxo de Operações
O processo foi desenvolvido utilizando a biblioteca medmnist, juntamente com bibliotecas auxiliares como opencv, matplotlib, e numpy, que proporcionaram as funções necessárias para o processamento e visualização das imagens. Abaixo estão as etapas detalhadas realizadas, que seguem as etapas determinada pelo enunciado do trabalho:

2.1. Carregamento e Preparação do Dataset
O dataset ChestMNIST foi carregado a partir da biblioteca medmnist, com a divisão split="test", para realizar os testes com as imagens. Para testar a robustez do sistema, o código permite a troca entre os datasets ChestMNIST e PneumoniaMNIST.
dataset = ChestMNIST(split="test", download=True)

2.2. Adição de Ruído
Uma das etapas iniciais consistiu na adição de ruído do tipo sal e pimenta às imagens. Foi utilizado o parâmetro amount=0.2 para adicionar 20% de pixels ruidosos à imagem original.
img_noise = add_salt_pepper_noise(img, amount=0.2)  # 20% de ruído
As imagens ruidosas foram exibidas lado a lado com as imagens originais para comparação visual.

2.3. Análise de Histograma
O histograma da imagem original (com ruído) foi calculado, seguido pela normalização para obter a função de densidade de probabilidade (PDF) e a função de distribuição cumulativa (CDF). Estes histogramas permitiram entender a distribuição de intensidade da imagem.

point_count, point_edges = np.histogram(img_noise, num_pontos, intervalo_min_max)
pdf = point_count / np.sum(point_count)
cdf = np.cumsum(pdf)
A equalização do histograma foi realizada a partir da CDF, e a imagem foi transformada para melhorar o contraste visual.

2.4. Equalização de Imagem
A imagem foi equalizada utilizando a CDF obtida no passo anterior. A transformação foi aplicada, e a imagem resultante foi comparada com a imagem original para visualizar a diferença.
img_eq = f_eq[img]

2.5. Correção Gama
A correção gama foi aplicada para ajustar a intensidade das imagens e melhorar a percepção visual das áreas escuras. O fator gama foi ajustado para 1.4, e a imagem resultante foi visualizada.

img_out = np.array(c*255*(img_in/255)**gamma, dtype=np.uint8)
2.6. Filtros de Suavização
Filtros de suavização (Gaussiano e Mediana) foram aplicados para reduzir o ruído da imagem. Três tamanhos de kernels foram utilizados para ambos os filtros (3x3, 5x5, 7x7), e as imagens resultantes foram exibidas para análise comparativa.

Filtro Gaussiano:
gaus_3x3 = gauss_create(sigma=1, size_x=3, size_y=3)
img_g3 = conv2d(img_out, gaus_3x3)

Filtro Mediana:
img_med3 = medianFilter(img_out, med_3x3, padding=True)
2.7. Realce de Bordas
Para realce das bordas, foi utilizado o filtro Laplaciano. A imagem foi aprimorada usando tanto o filtro Laplaciano aplicado à imagem original quanto o filtro aplicado a imagens suavizadas por Gaussiano e Mediana. O efeito foi visualizado através das imagens resultantes.

laplacianomed = conv2d_sharpening(img_med3, kernel_laplaciano)
laplace_final = img_out - laplace

2.8. Sobel e Realce de Bordas
O filtro de Sobel assim como laplaciano foi aplicado para detectar bordas verticais e horizontais. As imagens resultantes foram combinadas e normalizadas para realçar as bordas da imagem.

img_sobel_1 = conv2d_sharpening(img_med3, kernel_sobel_1)
img_sobel_2 = conv2d_sharpening(img_med3, kernel_sobel_2)
A combinação dos resultados do filtro de Sobel e da subtração de bordas foi realizada para aprimorar a imagem.

2.9. High-Boost Filtering
A técnica de High-Boost foi aplicada utilizando uma combinação entre a imagem original e a imagem suavizada, com o objetivo de aumentar as áreas de alta frequência (detalhes) da imagem. O fator A foi ajustado para controlar a intensidade do realce.

highboost = ((1+A) * original.astype(float)) - (A * suavizada.astype(float))

2.10. Visualização Completa do Pipeline
Por fim, uma visualização completa do pipeline foi gerada, mostrando todas as etapas do processamento da imagem, desde a adição de ruído até o realce final.

imagens_pipeline = { "Gamma Corrigida": img_out, "Gaussiano 3x3": img_g3, ...}
As imagens foram exibidas lado a lado, permitindo uma análise comparativa de cada etapa do processo.

3. Resultados
A seguir estão algumas imagens representativas dos resultados obtidos:

Imagem com Ruído: A imagem original com 20% de ruído do tipo sal e pimenta.

Imagem Equalizada: A imagem após a equalização do histograma, mostrando um contraste aprimorado.

Imagem com Filtro Gaussiano 3x3: A suavização da imagem com um filtro Gaussiano 3x3.

Imagem com Filtro Mediana 3x3: A suavização da imagem com um filtro Mediana 3x3.

Imagem com Realce de Bordas (Laplace): A imagem realçada com o filtro Laplaciano.

Imagem de High-Boost: A imagem final após a aplicação do filtro High-Boost, com aumento das áreas de alta frequência.

4. Conclusão
O processamento realizado demonstrou a eficácia de diferentes técnicas para melhorar a qualidade visual de imagens ruidosas. A combinação de equalização de histograma, correção gama, filtros de suavização, realce de bordas e High-Boost resultou em imagens de melhor qualidade, com maior nitidez e contraste. As técnicas de filtragem de bordas, como Sobel e Laplaciano, foram particularmente úteis para destacar os contornos das imagens.

