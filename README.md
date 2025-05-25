<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>📝 Quiz - Cartório</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary: #2c3e50;
            --primary-dark: #1a252f;
            --secondary: #3498db;
            --accent: #e74c3c;
            --correct: #2ecc71;
            --incorrect: #e74c3c;
            --light: #f8f9fa;
            --dark: #343a40;
            --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            --transition: all 0.3s ease;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            padding: 2rem;
            color: var(--dark);
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
        }
        
        /* Header Moderno */
        .quiz-header {
            text-align: center;
            margin-bottom: 2.5rem;
            animation: fadeInDown 0.8s;
        }
        
        .quiz-header h1 {
            font-size: 2.5rem;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 0.5rem;
            position: relative;
            display: inline-block;
        }
        
        .quiz-header h1::after {
            content: "";
            position: absolute;
            bottom: -10px;
            left: 50%;
            transform: translateX(-50%);
            width: 80px;
            height: 4px;
            background: var(--secondary);
            border-radius: 2px;
        }
        
        .quiz-header .subtitle {
            font-size: 1.1rem;
            color: var(--primary);
            max-width: 600px;
            margin: 0 auto;
            padding: 0.5rem;
            background-color: rgba(255, 255, 255, 0.7);
            border-radius: 20px;
        }
        
        /* Cartão Principal */
        .quiz-card {
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            margin-bottom: 2rem;
            transition: transform 0.3s;
        }
        
        .quiz-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.15);
        }
        
        /* Barra de Progresso */
        .progress-container {
            width: 100%;
            height: 8px;
            background: #e9ecef;
            position: relative;
        }
        
        .progress-bar {
            height: 100%;
            background: linear-gradient(90deg, var(--secondary), var(--primary));
            width: 0%;
            transition: width 0.4s ease;
            border-radius: 5px;
        }
        
        /* Perguntas */
        .question {
            padding: 2rem;
            display: none;
        }
        
        .question.active {
            display: block;
            animation: fadeIn 0.5s;
        }
        
        .question-number {
            display: inline-block;
            background: var(--primary);
            color: white;
            padding: 0.3rem 1rem;
            border-radius: 20px;
            font-size: 0.9rem;
            margin-bottom: 1rem;
            font-weight: 600;
        }
        
        .question-text {
            font-size: 1.3rem;
            font-weight: 600;
            margin-bottom: 1.5rem;
            color: var(--primary-dark);
        }
        
        /* Opções */
        .options {
            display: grid;
            gap: 1rem;
        }
        
        .option {
            background: white;
            border: 2px solid #e9ecef;
            border-radius: 10px;
            padding: 1rem 1.5rem;
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            font-weight: 500;
        }
        
        .option:hover {
            border-color: var(--secondary);
            background: #f8f9fa;
            transform: translateX(5px);
        }
        
        .option input {
            margin-right: 1rem;
            transform: scale(1.2);
            cursor: pointer;
        }
        
        /* Navegação */
        .navigation {
            display: flex;
            justify-content: space-between;
            margin-top: 2rem;
        }
        
        .nav-btn {
            padding: 0.8rem 1.5rem;
            border: none;
            border-radius: 50px;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
        }
        
        .nav-btn.prev {
            background: #f8f9fa;
            color: var(--primary);
        }
        
        .nav-btn.next {
            background: var(--secondary);
            color: white;
        }
        
        .nav-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        
        /* Resultado */
        .result-container {
            text-align: center;
            padding: 2rem;
            display: none;
        }
        
        .result-card {
            background: white;
            border-radius: 15px;
            padding: 2rem;
            box-shadow: var(--shadow);
            animation: fadeIn 0.5s;
        }
        
        .score-display {
            font-size: 2.5rem;
            font-weight: 700;
            margin: 1rem 0;
            background: linear-gradient(90deg, var(--secondary), var(--primary));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        
        .feedback {
            padding: 1.5rem;
            margin-top: 1.5rem;
            border-radius: 10px;
            display: none;
            animation: fadeIn 0.5s;
            text-align: left;
        }
        
        .feedback i {
            margin-right: 0.5rem;
        }
        
        .feedback.correct {
            background: rgba(46, 204, 113, 0.1);
            border-left: 5px solid var(--correct);
        }
        
        .feedback.incorrect {
            background: rgba(231, 76, 60, 0.1);
            border-left: 5px solid var(--incorrect);
        }
        
        /* Animações */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes fadeInDown {
            from { opacity: 0; transform: translateY(-30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        /* Responsivo */
        @media (max-width: 768px) {
            body {
                padding: 1rem;
            }
            
            .quiz-header h1 {
                font-size: 2rem;
            }
            
            .question {
                padding: 1.5rem;
            }
            
            .question-text {
                font-size: 1.1rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header class="quiz-header">
            <h1><i class="fas fa-file-contract"></i> Quiz - Cartório</h1>
            <p class="subtitle">Teste seus conhecimentos: Acerte 20+ e ganhe um certificado fictício de 'Mestre do Cartorio'🎓🖨️
 </p>
        </header>
        
        <div class="quiz-card">
            <div class="progress-container">
                <div class="progress-bar" id="progressBar"></div>
            </div>
            
            <form id="quizForm">
                <!-- As questões serão inseridas aqui pelo JavaScript -->
            </form>
            
            <div class="result-container" id="resultContainer">
                <div class="result-card">
                    <h2>Resultado Final</h2>
                    <div class="score-display" id="finalScore"></div>
                    <p id="resultMessage"></p>
                    <button class="btn" id="restartBtn">Refazer Quiz <i class="fas fa-redo"></i></button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Todas as 25 questões do quiz
        const questions = [
            {
                id: 1,
                question: "Qual é a principal diferença entre cartórios judiciais e extrajudiciais?",
                options: [
                    { text: "Judiciais realizam casamentos, extrajudiciais fazem divórcios", value: "a" },
                    { text: "Judiciais auxiliam o Poder Judiciário, extrajudiciais prestam serviços ao público", value: "b", correct: true },
                    { text: "Não há diferença, os termos são sinônimos", value: "c" },
                    { text: "Judiciais cuidam de imóveis, extrajudiciais de veículos", value: "d" }
                ],
                feedback: "Judiciais auxiliam o Poder Judiciário em processos judiciais, enquanto extrajudiciais (também chamados de serviços notariais e de registro) prestam serviços ao público em geral."
            },
            {
                id: 2,
                question: "Qual documento NÃO é tipicamente lavrado em um Cartório de Notas?",
                options: [
                    { text: "Escritura pública de compra e venda", value: "a" },
                    { text: "Procuração pública", value: "b" },
                    { text: "Registro de matrícula de imóvel", value: "c", correct: true },
                    { text: "Testamento público", value: "d" }
                ],
                feedback: "Registro de matrícula de imóvel - esta é função do Cartório de Registro de Imóveis, enquanto o Cartório de Notas lida com a lavratura de documentos como escrituras públicas, procurações e testamentos."
            },
            {
                id: 3,
                question: "Qual é o prazo máximo de validade para uma certidão de casamento apresentada para lavratura de escritura?",
                options: [
                    { text: "30 dias", value: "a" },
                    { text: "60 dias", value: "b" },
                    { text: "90 dias", value: "c", correct: true },
                    { text: "180 dias", value: "d" }
                ],
                feedback: "90 dias - Certidões de registro civil (nascimento, casamento) devem estar atualizadas, emitidas há no máximo 90 dias."
            },
            {
                id: 4,
                question: "Qual destes NÃO é um tipo de reconhecimento de firma?",
                options: [
                    { text: "Por semelhança", value: "a" },
                    { text: "Por autenticidade", value: "b" },
                    { text: "Por procuração", value: "c", correct: true },
                    { text: "Por verdadeiro", value: "d" }
                ],
                feedback: "Por procuração - Os tipos de reconhecimento são por semelhança (comparação com ficha de firma) e por autenticidade/verdadeiro (comparecimento pessoal)."
            },
            {
                id: 5,
                question: "Qual documento é obrigatório para escritura de compra e venda de imóvel urbano?",
                options: [
                    { text: "Certidão de Matrícula atualizada (30 dias)", value: "a", correct: true },
                    { text: "Certidão de Casamento dos envolvidos", value: "b" },
                    { text: "Comprovante de renda do comprador", value: "c" },
                    { text: "Contrato de financiamento aprovado", value: "d" }
                ],
                feedback: "Certidão de Matrícula atualizada (30 dias) - É documento essencial para qualquer transação com imóveis, com validade de 30 dias."
            },
            {
                id: 6,
                question: "No regime de comunhão parcial de bens, o que acontece com os bens adquiridos antes do casamento?",
                options: [
                    { text: "Tornam-se comuns ao casal", value: "a" },
                    { text: "Permanecem como propriedade particular", value: "b", correct: true },
                    { text: "Devem ser registrados em cartório", value: "c" },
                    { text: "São automaticamente doados ao cônjuge", value: "d" }
                ],
                feedback: "Permanecem como propriedade particular - No regime de comunhão parcial, os bens anteriores ao casamento permanecem particulares (art. 1.658 e 1.659 CC)."
            },
            {
                id: 7,
                question: "O que é necessário para que um documento estrangeiro seja aceito em cartório brasileiro?",
                options: [
                    { text: "Apenas tradução juramentada", value: "a" },
                    { text: "Legalização/apostila + tradução juramentada + registro no RTD", value: "b", correct: true },
                    { text: "Assinatura do cônsul do país de origem", value: "c" },
                    { text: "Autenticação por dois tabeliães", value: "d" }
                ],
                feedback: "Legalização/apostila + tradução juramentada + registro no RTD - Documentos estrangeiros precisam passar por esses três passos para produzir efeitos no Brasil."
            },
            {
                id: 8,
                question: "Qual sistema permite consultar a indisponibilidade de bens?",
                options: [
                    { text: "SIPLAN", value: "a" },
                    { text: "CNJ - https://indisponibilidade.cnj.jus.br", value: "b", correct: true },
                    { text: "Receita Federal", value: "c" },
                    { text: "Bacen", value: "d" }
                ],
                feedback: "CNJ - https://indisponibilidade.cnj.jus.br - É o sistema oficial para consulta de indisponibilidade de bens determinada pelo Poder Judiciário."
            },
            {
                id: 9,
                question: "Qual é a expressão típica usada ao final de atos notariais?",
                options: [
                    { text: "\"Para que conste\"", value: "a" },
                    { text: "\"O referido é verdade e dou fé\"", value: "b", correct: true },
                    { text: "\"Certifico e confirmo\"", value: "c" },
                    { text: "\"Assim está correto\"", value: "d" }
                ],
                feedback: "\"O referido é verdade e dou fé\" - Esta expressão afirma a presunção de veracidade dos atos notariais pela fé pública investida no tabelião."
            },
            {
                id: 10,
                question: "Qual destes NÃO é um dever ético do tabelião ou escrevente?",
                options: [
                    { text: "Sigilo profissional", value: "a" },
                    { text: "Imparcialidade", value: "b" },
                    { text: "Divulgar detalhes de atos em redes sociais", value: "c", correct: true },
                    { text: "Responsabilidade civil e criminal", value: "d" }
                ],
                feedback: "Divulgar detalhes de atos em redes sociais - O sigilo profissional é fundamental, e a divulgação de informações não autorizadas configura violação ética."
            },
            {
                id: 11,
                question: "Qual é o prazo para registro de um nascimento no cartório?",
                options: [
                    { text: "5 dias úteis", value: "a" },
                    { text: "15 dias", value: "b", correct: true },
                    { text: "30 dias", value: "c" },
                    { text: "Não há prazo legal", value: "d" }
                ],
                feedback: "15 dias - Conforme a Lei de Registros Públicos (Lei 6.015/73), o registro deve ser feito dentro de 15 dias do nascimento."
            },
            {
                id: 12,
                question: "Qual documento comprova a capacidade civil de uma pessoa para atos notariais?",
                options: [
                    { text: "RG", value: "a" },
                    { text: "CPF", value: "b" },
                    { text: "Certidão de nascimento ou casamento", value: "c", correct: true },
                    { text: "Certidão negativa de débitos", value: "d" }
                ],
                feedback: "Certidão de nascimento ou casamento - Esses documentos comprovam a capacidade civil, idade e estado civil da pessoa."
            },
            {
                id: 13,
                question: "O que é uma averbação em matrícula de imóvel?",
                options: [
                    { text: "Cancelamento da matrícula", value: "a" },
                    { text: "Anotação marginal que altera a situação jurídica do imóvel", value: "b", correct: true },
                    { text: "Transferência de propriedade", value: "c" },
                    { text: "Regularização de área", value: "d" }
                ],
                feedback: "Anotação marginal que altera a situação jurídica do imóvel - Como ônus reais, usufruto, hipoteca, etc."
            },
            {
                id: 14,
                question: "Qual é a diferença entre escritura pública e contrato particular?",
                options: [
                    { text: "A escritura é sempre mais cara", value: "a" },
                    { text: "O contrato particular não precisa de testemunhas", value: "b" },
                    { text: "A escritura tem fé pública e é título executivo extrajudicial", value: "c", correct: true },
                    { text: "Não há diferença jurídica", value: "d" }
                ],
                feedback: "A escritura tem fé pública e é título executivo extrajudicial - Ela goza de presunção de veracidade e autenticidade."
            },
            {
                id: 15,
                question: "Qual destes documentos NÃO pode ser registrado no Cartório de Títulos e Documentos?",
                options: [
                    { text: "Contrato de locação", value: "a" },
                    { text: "Contrato de compra e venda de veículo", value: "b" },
                    { text: "Procuração particular", value: "c" },
                    { text: "Escritura pública", value: "d", correct: true }
                ],
                feedback: "Escritura pública - As escrituras são registradas no próprio Cartório de Notas onde foram lavradas."
            },
            {
                id: 16,
                question: "O que é necessário para registrar um óbito?",
                options: [
                    { text: "Apenas a declaração de óbito", value: "a" },
                    { text: "Declaração de óbito e documento de identificação do declarante", value: "b" },
                    { text: "Declaração de óbito, documento do falecido e do declarante", value: "c", correct: true },
                    { text: "Nenhum documento, apenas o comparecimento de familiar", value: "d" }
                ],
                feedback: "Declaração de óbito, documento do falecido e do declarante - É necessário apresentar a DO (emitida por médico ou IML) e documentos de identificação."
            },
            {
                id: 17,
                question: "Qual é o efeito do registro de um contrato no Cartório de Títulos e Documentos?",
                options: [
                    { text: "Torna o contrato automáticamente válido", value: "a" },
                    { text: "Confere data certa e previne alegação de falsidade", value: "b", correct: true },
                    { text: "Dispensa a necessidade de testemunhas", value: "c" },
                    { text: "Substitui a escritura pública", value: "d" }
                ],
                feedback: "Confere data certa e previne alegação de falsidade - O registro dá publicidade e autenticidade ao documento."
            },
            {
                id: 18,
                question: "O que é uma certidão de inteiro teor?",
                options: [
                    { text: "Certidão resumida", value: "a" },
                    { text: "Cópia integral do registro original", value: "b", correct: true },
                    { text: "Certidão com valor internacional", value: "c" },
                    { text: "Certidão digitalizada", value: "d" }
                ],
                feedback: "Cópia integral do registro original - Reproduz fielmente todo o conteúdo do assento registral."
            },
            {
                id: 19,
                question: "Qual é a validade de uma procuração pública?",
                options: [
                    { text: "1 ano", value: "a" },
                    { text: "3 anos", value: "b" },
                    { text: "5 anos", value: "c" },
                    { text: "Depende do prazo estabelecido no instrumento", value: "d", correct: true }
                ],
                feedback: "Depende do prazo estabelecido no instrumento - Se não houver prazo determinado, vale enquanto não for revogada."
            },
            {
                id: 20,
                question: "O que é necessário para registrar um casamento?",
                options: [
                    { text: "Apenas a presença dos noivos", value: "a" },
                    { text: "Licença de casamento, documentos pessoais e testemunhas", value: "b", correct: true },
                    { text: "Certidão de nascimento e CPF", value: "c" },
                    { text: "Prova de residência e comprovante de renda", value: "d" }
                ],
                feedback: "Licença de casamento, documentos pessoais e testemunhas - São necessários documentos de identificação, certidões de estado civil e duas testemunhas maiores."
            },
            {
                id: 21,
                question: "Qual é o prazo para protesto de títulos?",
                options: [
                    { text: "3 dias úteis do vencimento", value: "a", correct: true },
                    { text: "30 dias do vencimento", value: "b" },
                    { text: "3 meses do vencimento", value: "c" },
                    { text: "1 ano do vencimento", value: "d" }
                ],
                feedback: "3 dias úteis do vencimento - O protesto deve ser requerido dentro deste prazo (Lei 9.492/97)."
            },
            {
                id: 22,
                question: "O que é um tabelionato de notas?",
                options: [
                    { text: "Cartório que registra imóveis", value: "a" },
                    { text: "Cartório que lavra escrituras e autentica documentos", value: "b", correct: true },
                    { text: "Cartório que registra nascimentos e óbitos", value: "c" },
                    { text: "Cartório que protesta títulos", value: "d" }
                ],
                feedback: "Cartório que lavra escrituras e autentica documentos - É o antigo nome do Cartório de Notas."
            },
            {
                id: 23,
                question: "Qual é a função principal do Cartório de Registro Civil?",
                options: [
                    { text: "Registrar imóveis", value: "a" },
                    { text: "Registrar nascimentos, casamentos e óbitos", value: "b", correct: true },
                    { text: "Lavrar escrituras", value: "c" },
                    { text: "Protestar títulos", value: "d" }
                ],
                feedback: "Registrar nascimentos, casamentos e óbitos - Também registra emancipações e interdições."
            },
            {
                id: 24,
                question: "O que é uma escritura de inventário e partilha?",
                options: [
                    { text: "Documento que declara bens para imposto de renda", value: "a" },
                    { text: "Documento que formaliza a divisão de bens de herança", value: "b", correct: true },
                    { text: "Registro de propriedade intelectual", value: "c" },
                    { text: "Contrato de doação em vida", value: "d" }
                ],
                feedback: "Documento que formaliza a divisão de bens de herança - Pode ser feito extrajudicialmente quando há consenso entre os herdeiros."
            },
            {
                id: 25,
                question: "Qual é a diferença entre averbação e registro de imóvel?",
                options: [
                    { text: "Averbação é definitiva, registro é temporário", value: "a" },
                    { text: "Registro cria direitos, averbação apenas anota alterações", value: "b", correct: true },
                    { text: "Não há diferença", value: "c" },
                    { text: "Averbação é para imóveis urbanos, registro para rurais", value: "d" }
                ],
                feedback: "Registro cria direitos, averbação apenas anota alterações - O registro transfere propriedade, a averbação apenas anota ônus ou alterações."
            }
        ];

        // Configurações do Quiz
        const quizConfig = {
            totalQuestions: questions.length,
            passingScore: 20,
            showAnswers: true
        };
        
        // Variáveis de estado
        let currentQuestionIndex = 0;
        let score = 0;
        let userAnswers = {};
        
        // Elementos DOM
        const quizForm = document.getElementById('quizForm');
        const progressBar = document.getElementById('progressBar');
        const resultContainer = document.getElementById('resultContainer');
        const finalScore = document.getElementById('finalScore');
        const resultMessage = document.getElementById('resultMessage');
        const restartBtn = document.getElementById('restartBtn');
        
        // Inicialização
        document.addEventListener('DOMContentLoaded', () => {
            renderQuestion(currentQuestionIndex);
            updateProgressBar();
        });
        
        // Renderiza uma questão específica
        function renderQuestion(index) {
            // Limpa o formulário
            quizForm.innerHTML = '';
            
            const question = questions[index];
            
            // Cria o elemento da questão
            const questionElement = document.createElement('div');
            questionElement.className = `question ${index === currentQuestionIndex ? 'active' : ''}`;
            questionElement.id = `question-${question.id}`;
            
            // Adiciona o número da pergunta
            const questionNumber = document.createElement('span');
            questionNumber.className = 'question-number';
            questionNumber.textContent = `Questão ${index + 1}/${quizConfig.totalQuestions}`;
            questionElement.appendChild(questionNumber);
            
            // Adiciona o texto da pergunta
            const questionText = document.createElement('p');
            questionText.className = 'question-text';
            questionText.textContent = question.question;
            questionElement.appendChild(questionText);
            
            // Adiciona as opções
            const optionsContainer = document.createElement('div');
            optionsContainer.className = 'options';
            
            question.options.forEach(option => {
                const optionId = `q${question.id}-${option.value}`;
                
                const optionLabel = document.createElement('label');
                optionLabel.className = 'option';
                
                const optionInput = document.createElement('input');
                optionInput.type = 'radio';
                optionInput.name = `q${question.id}`;
                optionInput.value = option.value;
                optionInput.id = optionId;
                
                // Marca como selecionado se já foi respondido
                if (userAnswers[question.id] === option.value) {
                    optionInput.checked = true;
                }
                
                optionLabel.appendChild(optionInput);
                
                const optionText = document.createTextNode(option.text);
                optionLabel.appendChild(optionText);
                
                optionsContainer.appendChild(optionLabel);
            });
            
            questionElement.appendChild(optionsContainer);
            
            // Adiciona o feedback (inicialmente oculto)
            const feedback = document.createElement('div');
            feedback.className = 'feedback';
            feedback.id = `feedback-${question.id}`;
            feedback.innerHTML = `<p><i class="fas fa-check-circle"></i> <strong>Resposta correta:</strong> ${question.feedback}</p>`;
            feedback.style.display = 'none';
            questionElement.appendChild(feedback);
            
            // Adiciona navegação
            const navigation = document.createElement('div');
            navigation.className = 'navigation';
            
            // Botão Anterior (não aparece na primeira questão)
            if (index > 0) {
                const prevBtn = document.createElement('button');
                prevBtn.type = 'button';
                prevBtn.className = 'nav-btn prev';
                prevBtn.innerHTML = '<i class="fas fa-arrow-left"></i> Anterior';
                prevBtn.addEventListener('click', () => {
                    navigateToQuestion(index - 1);
                });
                navigation.appendChild(prevBtn);
            }
            
            // Botão Próximo (ou Enviar na última questão)
            if (index < questions.length - 1) {
                const nextBtn = document.createElement('button');
                nextBtn.type = 'button';
                nextBtn.className = 'nav-btn next';
                nextBtn.innerHTML = 'Próxima <i class="fas fa-arrow-right"></i>';
                nextBtn.addEventListener('click', () => {
                    if (validateQuestion(question.id)) {
                        navigateToQuestion(index + 1);
                    }
                });
                navigation.appendChild(nextBtn);
            } else {
                const submitBtn = document.createElement('button');
                submitBtn.type = 'button';
                submitBtn.className = 'nav-btn next';
                submitBtn.innerHTML = 'Enviar <i class="fas fa-check"></i>';
                submitBtn.addEventListener('click', () => {
                    if (validateQuestion(question.id)) {
                        submitQuiz();
                    }
                });
                navigation.appendChild(submitBtn);
            }
            
            questionElement.appendChild(navigation);
            
            // Adiciona a questão ao formulário
            quizForm.appendChild(questionElement);
            
            // Atualiza a barra de progresso
            updateProgressBar();
        }
        
        // Navega para uma questão específica
        function navigateToQuestion(index) {
            // Salva a resposta atual
            saveAnswer();
            
            // Atualiza o índice da questão atual
            currentQuestionIndex = index;
            
            // Renderiza a nova questão
            renderQuestion(currentQuestionIndex);
        }
        
        // Valida se a questão foi respondida
        function validateQuestion(questionId) {
            const selectedOption = document.querySelector(`input[name="q${questionId}"]:checked`);
            
            if (!selectedOption) {
                alert('Por favor, selecione uma resposta antes de continuar.');
                return false;
            }
            
            return true;
        }
        
        // Salva a resposta do usuário
        function saveAnswer() {
            const question = questions[currentQuestionIndex];
            const selectedOption = document.querySelector(`input[name="q${question.id}"]:checked`);
            
            if (selectedOption) {
                userAnswers[question.id] = selectedOption.value;
            }
        }
        
        // Atualiza a barra de progresso
        function updateProgressBar() {
            const progress = ((currentQuestionIndex + 1) / quizConfig.totalQuestions) * 100;
            progressBar.style.width = `${progress}%`;
        }
        
        // Submete o quiz
        function submitQuiz() {
            // Salva a última resposta
            saveAnswer();
            
            // Calcula a pontuação
            calculateScore();
            
            // Mostra o resultado
            showResults();
        }
        
        // Calcula a pontuação
        function calculateScore() {
            score = 0;
            questions.forEach(q => {
                if (userAnswers[q.id] && userAnswers[q.id] === getCorrectAnswer(q.id)) {
                    score++;
                }
            });
        }
        
        // Mostra os resultados
        function showResults() {
            finalScore.textContent = `${score}/${quizConfig.totalQuestions}`;
            
            if (score >= quizConfig.passingScore) {
                resultMessage.innerHTML = `
                    <div class="feedback correct">
                        <i class="fas fa-trophy"></i> <strong>Parabéns!</strong> Você é um verdadeiro Expert Cartorário!
                    </div>
                    <p>Você dominou os principais conceitos do universo cartorário.</p>
                `;
            } else {
                resultMessage.innerHTML = `
                    <div class="feedback incorrect">
                        <i class="fas fa-book"></i> <strong>Quase lá!</strong> Recomendamos revisar alguns tópicos.
                    </div>
                    <p>Continue estudando para se tornar um mestre dos cartórios!</p>
                `;
            }
            
            // Mostra feedback das respostas se configurado
            if (quizConfig.showAnswers) {
                showAnswersFeedback();
            }
            
            quizForm.style.display = 'none';
            resultContainer.style.display = 'block';
        }
        
        // Mostra o feedback para todas as respostas
        function showAnswersFeedback() {
            questions.forEach(q => {
                const feedbackElement = document.getElementById(`feedback-${q.id}`);
                if (feedbackElement) {
                    feedbackElement.style.display = 'block';
                    
                    // Destaca a resposta correta
                    const correctOption = document.querySelector(`input[name="q${q.id}"][value="${getCorrectAnswer(q.id)}"]`);
                    if (correctOption && correctOption.parentElement) {
                        correctOption.parentElement.style.backgroundColor = "rgba(46, 204, 113, 0.2)";
                        correctOption.parentElement.style.borderColor = "#2ecc71";
                    }
                    
                    // Destaca a resposta do usuário se estiver errada
                    if (userAnswers[q.id] && userAnswers[q.id] !== getCorrectAnswer(q.id)) {
                        const userOption = document.querySelector(`input[name="q${q.id}"][value="${userAnswers[q.id]}"]`);
                        if (userOption && userOption.parentElement) {
                            userOption.parentElement.style.backgroundColor = "rgba(231, 76, 60, 0.2)";
                            userOption.parentElement.style.borderColor = "#e74c3c";
                        }
                    }
                }
            });
        }
        
        // Obtém a resposta correta para uma questão
        function getCorrectAnswer(questionId) {
            const question = questions.find(q => q.id === questionId);
            return question.options.find(opt => opt.correct).value;
        }
        
        // Botão de reiniciar
        restartBtn.addEventListener('click', () => {
            location.reload();
        });
    </script>
</body>
</html>
