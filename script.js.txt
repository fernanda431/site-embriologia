// Loader
window.addEventListener('load', function () {
  const loader = document.getElementById('loader');
  const content = document.getElementById('content');
  setTimeout(() => {
    loader.style.display = 'none';
    content.classList.remove('hidden');
  }, 1500);
});

// Menu toggle
function toggleMenu() {
  const menu = document.getElementById('menu');
  menu.classList.toggle('expanded');
}

// Frases de curiosidades
const frases = [
  "O coração começa a bater entre a 3ª e 4ª semana.",
  "O tubo neural se fecha por volta da 4ª semana.",
  "Na 5ª semana, os olhos e ouvidos começam a se formar.",
  "Entre a 6ª e 8ª semana, os dedos começam a se separar.",
  "O embrião dobra de tamanho a cada semana no início!"
];

document.addEventListener('DOMContentLoaded', () => {
  const curiosidade = document.getElementById('curiosidade');
  const fraseAleatoria = frases[Math.floor(Math.random() * frases.length)];
  curiosidade.textContent = `"${fraseAleatoria}"`;
});
const quizData = [
  {
    pergunta: "Em qual semana o coração começa a bater no embrião?",
    opcoes: ["2ª semana", "3ª semana", "5ª semana", "6ª semana"],
    correta: 1,
    explicacao: "O coração começa a bater por volta da 3ª semana de gestação."
  },
  {
    pergunta: "O tubo neural se fecha geralmente em qual semana?",
    opcoes: ["2ª semana", "4ª semana", "6ª semana", "8ª semana"],
    correta: 1,
    explicacao: "O fechamento do tubo neural ocorre por volta da 4ª semana."
  },
  {
    pergunta: "Qual estrutura começa a se formar na 5ª semana?",
    opcoes: ["Olhos e ouvidos", "Coração", "Cérebro completo", "Pulmões"],
    correta: 0,
    explicacao: "Na 5ª semana, olhos e ouvidos estão começando a se desenvolver."
  },
  {
    pergunta: "Quando os dedos das mãos começam a se separar?",
    opcoes: ["2ª semana", "4ª semana", "6ª semana", "8ª semana"],
    correta: 2,
    explicacao: "Na 6ª semana ocorre a separação dos dedos das mãos."
  },
  {
    pergunta: "Na 8ª semana, o embrião já possui:",
    opcoes: ["Órgãos formados", "Rostos definidos", "Sistema digestivo funcional", "Batimentos cardíacos irregulares"],
    correta: 0,
    explicacao: "Na 8ª semana, a maioria dos órgãos já está formada."
  },
  {
    pergunta: "A cauda embrionária desaparece em qual semana?",
    opcoes: ["2ª", "4ª", "6ª", "7ª"],
    correta: 3,
    explicacao: "A cauda embrionária regride por volta da 7ª semana."
  },
  {
    pergunta: "A face começa a se formar em qual semana?",
    opcoes: ["3ª", "5ª", "6ª", "8ª"],
    correta: 1,
    explicacao: "A face começa a se formar na 5ª semana."
  },
  {
    pergunta: "O cérebro começa a se desenvolver a partir de:",
    opcoes: ["2ª semana", "3ª semana", "4ª semana", "5ª semana"],
    correta: 2,
    explicacao: "O cérebro começa a se formar na 4ª semana."
  },
  {
    pergunta: "As pernas e braços começam a aparecer na:",
    opcoes: ["3ª semana", "4ª semana", "6ª semana", "8ª semana"],
    correta: 1,
    explicacao: "Os membros começam a brotar na 4ª semana."
  },
  {
    pergunta: "Qual o tamanho aproximado do embrião na 8ª semana?",
    opcoes: ["0,5 cm", "1 cm", "3 cm", "5 cm"],
    correta: 2,
    explicacao: "Na 8ª semana o embrião tem cerca de 3 cm."
  }
];

let perguntaAtual = 0;
let acertos = 0;

const questionText = document.getElementById("question-text");
const optionsContainer = document.getElementById("options");
const feedback = document.getElementById("feedback");
const nextBtn = document.getElementById("next-btn");
const quizBox = document.getElementById("quiz-box");
const quizEnd = document.getElementById("quiz-end");
const resultTitle = document.getElementById("result-title");
const resultMsg = document.getElementById("result-msg");
const progressBar = document.getElementById("progress-bar");

const correctSound = document.getElementById("correct-sound");
const wrongSound = document.getElementById("wrong-sound");

function carregarPergunta() {
  const atual = quizData[perguntaAtual];
  questionText.textContent = atual.pergunta;
  optionsContainer.innerHTML = "";
  feedback.textContent = "";
  nextBtn.style.display = "none";

  const progresso = ((perguntaAtual) / quizData.length) * 100;
  progressBar.style.width = progresso + "%";

  atual.opcoes.forEach((opcao, index) => {
    const btn = document.createElement("button");
    btn.textContent = opcao;
    btn.className = "btn quiz-option";
    btn.onclick = () => verificarResposta(index);
    optionsContainer.appendChild(btn);
  });
}

function verificarResposta(escolhida) {
  const atual = quizData[perguntaAtual];
  const botoes = document.querySelectorAll(".quiz-option");
  botoes.forEach(btn => btn.disabled = true);

  if (escolhida === atual.correta) {
    botoes[escolhida].classList.add("correct");
    feedback.textContent = "✅ Correto! " + atual.explicacao;
    correctSound.play();
    acertos++;
  } else {
    botoes[escolhida].classList.add("incorrect");
    botoes[atual.correta].classList.add("correct");
    feedback.textContent = "❌ Errado! " + atual.explicacao;
    wrongSound.play();
  }

  nextBtn.style.display = "block";
}

nextBtn.onclick = () => {
  perguntaAtual++;
  if (perguntaAtual < quizData.length) {
    carregarPergunta();
  } else {
    finalizarQuiz();
  }
};

function finalizarQuiz() {
  quizBox.style.display = "none";
  quizEnd.classList.remove("hidden");

  const progresso = (acertos / quizData.length) * 100;
  progressBar.style.width = "100%";

  if (acertos >= 9) {
    resultTitle.textContent = "🏆 Perfeito!";
    resultMsg.textContent = "Nível máximo desbloqueado! Você é um mestre do desenvolvimento!";
  } else if (acertos >= 6) {
    resultTitle.textContent = "⚡ Muito bom!";
    resultMsg.textContent = "Quase lá! Continue jogando para virar um expert!";
  } else {
    resultTitle.textContent = "🔥 Tente novamente!";
    resultMsg.textContent = "A jornada do conhecimento continua. Você pode melhorar!";
  }
}

function restartQuiz() {
  perguntaAtual = 0;
  acertos = 0;
  quizBox.style.display = "block";
  quizEnd.classList.add("hidden");
  carregarPergunta();
}

// Só inicia o quiz se estiver na página de quiz
if (document.getElementById("quiz-box")) {
  carregarPergunta();
}
