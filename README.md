<div class="container">
  <h1>Gerador de Paletas</h1>
  <p>Pressione a <strong>Barra de Espaço</strong> para mudar as cores ou clique nelas para copiar!</p>
  
  <div class="palette" id="palette">
    </div>
  
  <button id="generate-btn">Gerar Nova Paleta</button>
</div>

<div class="toast" id="toast">Copiado para a área de transferência!</div>
box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
  background-color: #f0f2f5;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  color: #333;
}

.container {
  text-align: center;
  background: white;
  padding: 2.5rem;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  width: 90%;
  max-width: 800px;
}

h1 {
  margin-bottom: 0.5rem;
  font-size: 2rem;
}

p {
  color: #666;
  margin-bottom: 2rem;
  font-size: 0.95rem;
}

.palette {
  display: flex;
  gap: 15px;
  height: 250px;
  margin-bottom: 2rem;
}

.color-card {
  flex: 1;
  border-radius: 12px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: center;
  padding-bottom: 1.5rem;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  position: relative;
  overflow: hidden;
}

.color-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0,0,0,0.15);
}

.color-code {
  background: rgba(255, 255, 255, 0.85);
  padding: 0.4rem 0.8rem;
  border-radius: 20px;
  font-weight: 600;
  font-size: 0.85rem;
  letter-spacing: 1px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
}

button {
  background-color: #4f46e5;
  color: white;
  border: none;
  padding: 0.8rem 2rem;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.2s;
}

button:hover {
  background-color: #4338ca;
}

/* Notificação Toast */
.toast {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%) translateY(100px);
  background: #333;
  color: white;
  padding: 0.7rem 1.5rem;
  border-radius: 30px;
  font-size: 0.9rem;
  box-shadow: 0 5px 15px rgba(0,0,0,0.2);
  transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  opacity: 0;
  pointer-events: none;
}

.toast.show {
  transform: translateX(-50%) translateY(0);
  opacity: 1;
}
const paletteContainer = document.getElementById('palette');
const generateBtn = document.getElementById('generate-btn');
const toast = document.getElementById('toast');
const colorCount = 5; // Quantidade de cores na paleta

// Função para gerar uma cor hexadecimal aleatória
function generateRandomColor() {
  const chars = '0123456789ABCDEF';
  let color = '#';
  for (let i = 0; i < 6; i++) {
    color += chars[Math.floor(Math.random() * 16)];
  }
  return color;
}

// Função para criar os cards de cores na tela
function generatePalette() {
  paletteContainer.innerHTML = ''; // Limpa a paleta anterior
  
  for (let i = 0; i < colorCount; i++) {
    const randomColor = generateRandomColor();
    
    // Cria o elemento do card
    const colorCard = document.createElement('div');
    colorCard.classList.add('color-card');
    colorCard.style.backgroundColor = randomColor;
    
    // Cria o texto com o código hexadecimal
    const colorCode = document.createElement('span');
    colorCode.classList.add('color-code');
    colorCode.innerText = randomColor;
    
    colorCard.appendChild(colorCode);
    
    // Evento de clique para copiar a cor
    colorCard.addEventListener('click', () => copyToClipboard(randomColor));
    
    paletteContainer.appendChild(colorCard);
  }
}

// Função para copiar o código da cor
function copyToClipboard(text) {
  navigator.clipboard.writeText(text).then(() => {
    showToast();
  }).catch(err => {
    console.error('Erro ao copiar: ', err);
  });
}

// Exibe a notificação na tela
function showToast() {
  toast.classList.add('show');
  setTimeout(() => {
    toast.classList.remove('show');
  }, 2000);
}

// Eventos de ativação (Clique no botão e Tecla de Espaço)
generateBtn.addEventListener('click', generatePalette);

document.body.addEventListener('keydown', (e) => {
  if (e.code === 'Space') {
    e.preventDefault(); // Evita que a página role para baixo
    generatePalette();
  }
});

// Inicializa a primeira paleta ao carregar a página
generatePalette();
