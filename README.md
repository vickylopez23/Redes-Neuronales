# Redes Neuronales — FaMAF (UNC)

Trabajos de la materia optativa **Redes Neuronales** de la Licenciatura en Ciencias de la Computación (FaMAF, UNC). Todos usan PyTorch sobre el dataset **Fashion-MNIST**: imágenes de prendas en escala de grises, de 28x28 píxeles y 10 categorías.

Autora: Victoria López

| Trabajo | Tema | Resultado principal |
|---------|------|---------------------|
| [TP 2](tp2-fashion-mnist/) | Clasificador con red multicapa (MLP) | Análisis de hiperparámetros |
| [Final](final-autoencoders/) | Autoencoder convolucional y pre-entrenamiento | 88,3 % de accuracy en test (entrenamiento desde cero) |

---

## TP 2 — Clasificando Fashion-MNIST con PyTorch (2024)

Implementación en PyTorch de una red neuronal multicapa para clasificar las imágenes de prendas del dataset Fashion-MNIST.

- **Arquitectura:** entrada de 784 píxeles, dos capas ocultas de 128 y 64 neuronas con ReLU y dropout, y salida de 10 neuronas.
- **Entrenamiento:** pérdida de entropía cruzada, con optimizadores SGD y Adam.
- **Análisis:** efecto del tamaño de lote, la tasa de aprendizaje y la estructura de la red.

Informe: [`tp2-fashion-mnist/RedesNeuronalesTp2.pdf`](tp2-fashion-mnist/RedesNeuronalesTp2.pdf)

---

## Final — Autoencoders y clasificadores convolucionales (2026)

Se entrena un **autoencoder convolucional** sin etiquetas y después se reutiliza su encoder para **clasificar**, con el objetivo de medir cuánto aporta el pre-entrenamiento no supervisado.

### Autoencoder

- **Encoder:** convoluciones de 32, 64 y 128 filtros con max pooling. Espacio latente de **128 × 7 × 7**.
- **Decoder:** convoluciones transpuestas que reconstruyen la imagen de 28 × 28.
- **Entrenamiento:** MSE, Adam con lr = 1e-3, batch de 128 y 20 épocas. División 80 % train / 20 % validación; el set de test se reserva para la evaluación final.
- Las reconstrucciones conservan la forma y los contornos de las prendas y suavizan algunos detalles finos.

### Clasificador

El encoder se conecta a un cabezal convolucional: convolución, average pooling, dos capas densas (32 → 128 → 10) con ReLU y dropout, y LogSoftmax con NLLLoss. Se compararon cuatro esquemas de entrenamiento:

| Esquema | Val Acc | Test Acc |
|---------|--------:|---------:|
| Encoder pre-entrenado congelado | 82,81 % | 81,97 % |
| **Desde cero** | **88,51 %** | **88,30 %** |
| Fine-tuning del encoder | 85,43 % | 84,88 % |
| Encoder aleatorio congelado (línea base) | 73,84 % | 73,43 % |

### Conclusiones

- El encoder pre-entrenado supera claramente al aleatorio (81,97 % frente a 73,43 %): el autoencoder aprende representaciones útiles para clasificar.
- Con tantos datos etiquetados como tiene Fashion-MNIST, el entrenamiento completo desde cero es el que mejor rinde, y el fine-tuning queda en un punto intermedio.
- La clase más difícil es **Camisa** (35,9 % de precisión con el encoder congelado), porque se confunde con Camiseta, Pullover y Abrigo. Calzado y Bolso superan el 92 %.

Archivos:
- Informe: [`final-autoencoders/Informe_Final_Autoencoders.pdf`](final-autoencoders/Informe_Final_Autoencoders.pdf)
- Notebook: [`final-autoencoders/redes_neuronales_final.ipynb`](final-autoencoders/redes_neuronales_final.ipynb)
