<script lang="ts">
  // Importa ferramentas do Svelte (neste caso o onMount, embora não o estejamos usando no momento, é comum ter)
  import { onMount } from "svelte";

  // --- CONTROLE DE SCROLL (ROLAGEM DA TELA) ---
  // Criamos uma variável que vai guardar o número de pixels que o usuário já rolou para baixo.
  let scrollY = 0;

  // O símbolo "$:" é um superpoder do Svelte (chamado de reatividade).
  // Ele diz: "Toda vez que o scrollY mudar, recalcule esta variável pollutionOpacity automaticamente".
  // Math.min garante que o valor nunca passe de 1 (onde 0 é transparente e 1 é totalmente visível).
  // Aumentamos para 800 para a transição da poluição ficar mais suave acompanhando a nova altura da tela.
  $: pollutionOpacity = Math.min(scrollY / 800, 1);

  // --- DADOS DO JOGO ---
  // 'type' cria uma lista de opções permitidas. Assim, não corremos o risco de digitar o tipo de lixo errado.
  type TrashType = "plastico" | "papel" | "vidro" | "metal";

  // 'interface' funciona como um "molde" ou "contrato".
  // Dizemos ao código que todo 'TrashItem' (item de lixo) OBRIGATORIAMENTE precisa ter um id, nome, tipo e mensagem.
  interface TrashItem {
    id: number;
    name: string;
    type: TrashType;
    impactMessage: string;
  }

  // Molde para as lixeiras. Toda lixeira precisa de um tipo, uma cor e um nome.
  interface Bin {
    type: TrashType;
    color: string;
    name: string;
  }

  // Criamos a lista (Array) de lixeiras, seguindo o Padrão de Cores ABNT/CONAMA rigorosamente.
  const bins: Bin[] = [
    { type: "papel", color: "#0052cc", name: "Papel (Azul)" },
    { type: "plastico", color: "#dc2626", name: "Plástico (Vermelho)" },
    { type: "vidro", color: "#16a34a", name: "Vidro (Verde)" },
    { type: "metal", color: "#ca8a04", name: "Metal (Amarelo)" },
  ];

  // Criamos a lista inicial dos lixos que ficarão espalhados pelo chão.
  const initialTrashItems: TrashItem[] = [
    {
      id: 1,
      name: "Garrafa PET",
      type: "plastico",
      impactMessage:
        "Incrível! Reciclar plástico evita que animais marinhos se machuquem e economiza petróleo!",
    },
    {
      id: 2,
      name: "Caixa de Papelão",
      type: "papel",
      impactMessage:
        "Muito bem! Reciclar papel salva muitas árvores de serem cortadas!",
    },
    {
      id: 3,
      name: "Lata de Refrigerante",
      type: "metal",
      impactMessage:
        "Show! Reciclar alumínio economiza muita energia elétrica e minérios da natureza!",
    },
    {
      id: 4,
      name: "Pote de Geleia",
      type: "vidro",
      impactMessage:
        "Perfeito! O vidro pode ser reciclado infinitas vezes sem perder a qualidade!",
    },
  ];

  // 'trashItems' é a lista de lixos atual do jogo. Os "três pontinhos" (...initialTrashItems)
  // servem para fazer uma cópia exata da lista inicial, em vez de alterar a original.
  let trashItems: TrashItem[] = [...initialTrashItems];

  // Variável para controlar em qual fase o jogador está (1 = Fácil, 2 = Difícil sem texto).
  let currentRound = 1;

  // --- ESTADO DO MODAL (JANELINHA DE AVISO) ---
  // Variáveis para controlar se a janelinha de mensagem está aparecendo e o que está escrito nela.
  let modalVisible = false; // Começa escondido (false)
  let modalTitle = "";
  let modalMessage = "";
  let modalType: "success" | "error" | "finish" = "success"; // Define as cores do modal

  // --- LÓGICA DO JOGO (Arraste e Solte / Drag & Drop) ---
  // Variável que guarda o lixo que o usuário está segurando (arrastando) no momento.
  // Começa como 'null' (vazio) porque ninguém está arrastando nada no início.
  let draggedItem: TrashItem | null = null;

  // Função disparada no exato momento em que o usuário clica e começa a arrastar um lixo.
  function handleDragStart(item: TrashItem) {
    draggedItem = item; // Salva o lixo atual na variável draggedItem
  }

  // Função disparada quando o usuário solta o lixo em cima de uma lixeira.
  // Recebe como parâmetro o 'binType' (tipo da lixeira: plástico, papel, etc).
  function handleDrop(binType: TrashType) {
    // Se o usuário soltou sem estar arrastando nada, a função para por aqui (return).
    if (!draggedItem) return;

    // Verifica se o tipo do lixo arrastado é igual ao tipo da lixeira onde ele foi solto.
    if (draggedItem.type === binType) {
      // Se ACERTOU: Mostra a mensagem de sucesso com a curiosidade sobre aquele lixo.
      showModal("Parabéns!", draggedItem.impactMessage, "success");

      // Remove o lixo da tela filtrando a lista (mantém todos os lixos, exceto o que acabou de ser jogado fora).
      trashItems = trashItems.filter((item) => item.id !== draggedItem?.id);

      // Verifica se a lista de lixos ficou vazia (ou seja, se o jogador catou tudo).
      if (trashItems.length === 0) {
        if (currentRound === 1) {
          // Se estava na fase 1, espera 1 segundo e meio (1500ms) e avança para a fase 2.
          setTimeout(() => {
            showModal(
              "Nível 2 Desbloqueado!",
              "Agora vamos testar sua memória. As lixeiras perderam os textos, use apenas as cores para acertar!",
              "success",
            );
            currentRound = 2; // Muda para a fase 2
            trashItems = [...initialTrashItems]; // Reinicia os lixos no chão
          }, 1500);
        } else {
          // Se já estava na fase 2 e esvaziou, mostra a mensagem de vitória final.
          setTimeout(() => {
            showModal(
              "Você Salvou a Cidade!",
              "Todo o lixo foi recolhido e reciclado. Você é um verdadeiro herói do meio ambiente!",
              "finish",
            );
          }, 1500);
        }
      }
    } else {
      // Se ERROU: Mostra a janelinha de erro. O lixo não é removido da lista.
      showModal(
        "Ops! Lixeira Errada",
        `Esse item não vai nessa lixeira. Tente novamente!`,
        "error",
      );
    }
    // Ao final, quer acerte ou erre, esvazia a "mão" do jogador (ninguém está arrastando mais nada).
    draggedItem = null;
  }

  // Função auxiliar para configurar as informações do Modal e exibi-lo na tela (muda visível para true).
  function showModal(
    title: string,
    message: string,
    type: "success" | "error" | "finish",
  ) {
    modalTitle = title;
    modalMessage = message;
    modalType = type;
    modalVisible = true;
  }

  // Função para esconder o Modal (muda visível para false).
  function closeModal() {
    modalVisible = false;
  }
</script>

<svelte:window bind:scrollY />

<main>
  <section class="hero-section">
    <div class="city-layer clean-city"></div>

    <div
      class="city-layer polluted-city"
      style="opacity: {pollutionOpacity};"
    ></div>
  </section>

  <section class="content-section">
    <h2>O Retrato do Lixo no Brasil</h2>
    <p>
      Segundo dados da <strong>ABRELPE</strong> (Associação Brasileira de
      Empresas de Limpeza Pública e Resíduos Especiais), o Brasil gera cerca de
      <strong>82 milhões de toneladas</strong> de resíduos sólidos urbanos por ano.
      Desse total, uma grande parte ainda tem destinação inadequada, afetando diretamente
      o meio ambiente e as cidades.
    </p>

    <div class="info-cards">
      <div class="card card-azul">
        <h3 class="text-azul">Aumento Contínuo</h3>
        <p>
          A geração de lixo no Brasil cresce a cada ano, muitas vezes em ritmo
          superior ao crescimento populacional, exigindo ações urgentes.
        </p>
      </div>
      <div class="card card-vermelho">
        <h3 class="text-vermelho">Baixa Reciclagem</h3>
        <p>
          Apenas cerca de 4% dos resíduos sólidos que poderiam ser reciclados no
          país são efetivamente reaproveitados.
        </p>
      </div>
      <div class="card card-verde">
        <h3 class="text-verde">O Caminho</h3>
        <p>
          A educação ambiental e a separação correta dos resíduos nas
          residências são os primeiros passos para mudar essa realidade.
        </p>
      </div>
    </div>
  </section>

  <section class="impacts-section">
    <h2>Os Impactos do Lixo nas Cidades</h2>
    <p>
      O descarte inadequado de quase 30 milhões de toneladas anuais em lixões e
      aterros controlados gera consequências graves:
    </p>

    <div class="impact-container">
      <div class="impact-row">
        <div class="impact-text border-azul">
          <h3 class="text-azul">Enchentes e Infraestrutura</h3>
          <p>
            O lixo descartado incorretamente nas vias públicas é carregado pelas
            chuvas, entupindo bueiros e redes de drenagem. Isso é uma das
            principais causas de enchentes urbanas, gerando prejuízos materiais
            e paralisação das cidades.
          </p>
        </div>
        <div class="impact-text border-vermelho">
          <h3 class="text-vermelho">Saúde Pública</h3>
          <p>
            O acúmulo de lixo em locais inadequados atrai vetores de doenças,
            como ratos e mosquitos (incluindo o Aedes aegypti). Segundo a
            ABRELPE, milhões de brasileiros têm a saúde impactada por viverem
            próximos a áreas de descarte irregular.
          </p>
        </div>
        <div class="impact-text border-amarelo">
          <h3 class="text-amarelo">Poluição Ambiental e Climática</h3>
          <p>
            Lixões a céu aberto contaminam o solo e os lençóis freáticos através
            do chorume. Além disso, a decomposição orgânica inadequada emite
            grandes quantidades de gás metano, contribuindo fortemente para o
            aquecimento global.
          </p>
        </div>
      </div>
    </div>
  </section>

  <section class="lifecycle-section">
    <h2>O Ciclo e o Impacto de Cada Material</h2>
    <p>
      Entenda quanto tempo cada resíduo permanece no planeta e o que acontece
      quando é descartado de forma incorreta:
    </p>

    <div class="lifecycle-container">
      <div class="lifecycle-card border-vermelho">
        <h3 class="text-vermelho">Plástico (Centenas de anos)</h3>
        <p>
          <strong>Ciclo:</strong> Demora até 400 anos para se decompor. Quando no
          ambiente, fragmenta-se em microplásticos altamente tóxicos.
        </p>
        <p>
          <strong>Impactos:</strong> Nas cidades, é o principal causador de entupimento
          de bueiros, agravando as inundações. Na natureza, é ingerido por animais
          marinhos, causando mortes em massa e desequilíbrio ecológico.
        </p>
      </div>

      <div class="lifecycle-card border-azul">
        <h3 class="text-azul">Papel (Meses a anos)</h3>
        <p>
          <strong>Ciclo:</strong> Decompõe-se em alguns meses, mas sua produção exige
          o corte de milhares de árvores e gigantesco consumo de água.
        </p>
        <p>
          <strong>Impactos:</strong> O desperdício impulsiona o desmatamento. Quando
          enterrado em lixões, sua decomposição sem oxigênio libera altas taxas de
          gás metano, um dos maiores causadores do efeito estufa.
        </p>
      </div>

      <div class="lifecycle-card border-verde">
        <h3 class="text-verde">Vidro (Milhares de anos)</h3>
        <p>
          <strong>Ciclo:</strong> Pode levar mais de 4.000 anos para desaparecer
          na natureza, no entanto, é 100% e infinitamente reciclável.
        </p>
        <p>
          <strong>Impactos:</strong> Quando descartado em matas, seus cacos podem
          funcionar como uma lupa sob o sol e iniciar incêndios florestais devastadores.
          Também causa acidentes graves a animais e profissionais da coleta seletiva.
        </p>
      </div>

      <div class="lifecycle-card border-amarelo">
        <h3 class="text-amarelo">Metal (Centenas de anos)</h3>
        <p>
          <strong>Ciclo:</strong> O alumínio leva de 200 a 500 anos para se decompor.
          Sua extração da natureza é extremamente agressiva ao solo.
        </p>
        <p>
          <strong>Impactos:</strong> A oxidação de metais descartados inadequadamente
          contamina o solo e lençóis freáticos. A ausência de reciclagem exige mais
          mineração, que destrói grandes áreas florestais e contamina rios com rejeitos.
        </p>
      </div>
    </div>
  </section>

  <section class="three-rs-section">
    <h2>A Regra Mágica dos 3 R's</h2>
    <p>
      Para revertermos as estatísticas e sermos amigos do planeta, precisamos
      seguir três passos essenciais:
    </p>

    <div class="rs-container">
      <div class="r-card reduce border-vermelho">
        <h3 class="text-vermelho">1. Reduzir</h3>
        <p>
          Comprar apenas o que realmente precisamos e evitar desperdícios. Menos
          consumo desnecessário, menos geração de lixo!
        </p>
      </div>

      <div class="r-card reuse border-amarelo">
        <h3 class="text-amarelo">2. Reutilizar</h3>
        <p>
          Dar uma nova utilidade aos objetos antes de descartá-los. Prolongar a
          vida útil dos materiais diminui a pressão sobre os recursos naturais.
        </p>
      </div>

      <div class="r-card recycle border-verde">
        <h3 class="text-verde">3. Reciclar</h3>
        <p>
          Separar e destinar os materiais usados para que sejam transformados em
          novos produtos pelas indústrias de reciclagem.
        </p>
      </div>
    </div>
  </section>

  <section class="game-section">
    <h2>Hora de Agir!</h2>
    <p>
      Arraste o lixo espalhado para a lixeira correta e ajude a limpar a cidade!
    </p>

    <div class="trash-area border-verde">
      {#each trashItems as item (item.id)}
        <div
          class="trash-item"
          draggable="true"
          on:dragstart={() => handleDragStart(item)}
          aria-grabbed="false"
        >
          <span>{item.name}</span>
        </div>
      {/each}

      {#if trashItems.length === 0 && currentRound === 2}
        <div class="empty-trash-message">Área limpa! Excelente trabalho!</div>
      {/if}
    </div>

    <div class="bins-area">
      {#each bins as bin}
        <div
          class="bin"
          style="border-color: {bin.color};"
          on:dragover|preventDefault
          on:drop={() => handleDrop(bin.type)}
          role="region"
        >
          <div class="bin-cap" style="background-color: {bin.color};"></div>
          <div class="bin-body">
            {#if currentRound === 1}
              <strong>{bin.name}</strong>
            {/if}
          </div>
        </div>
      {/each}
    </div>
  </section>

  {#if modalVisible}
    <div
      class="modal-overlay"
      on:click={closeModal}
      on:keydown={closeModal}
      tabindex="0"
      role="button"
    >
      <div
        class="modal-content {modalType}"
        on:click|stopPropagation
        tabindex="0"
        role="dialog"
      >
        <h2>{modalTitle}</h2>
        <p>{modalMessage}</p>
        <button on:click={closeModal} class="modal-btn">Continuar</button>
      </div>
    </div>
  {/if}
</main>

<style>
  /* --- PALETA DE CORES ABNT (Variáveis Globais) --- */
  /* ':root' define variáveis que podemos reutilizar no código todo. Se precisar mudar a cor depois, muda só aqui. */
  :root {
    --abnt-azul: #0052cc; /* Cor oficial do Papel */
    --abnt-vermelho: #dc2626; /* Cor oficial do Plástico */
    --abnt-verde: #16a34a; /* Cor oficial do Vidro */
    --abnt-amarelo: #ca8a04; /* Cor oficial do Metal */

    /* Tons pasteis clarinhos para o fundo das seções para os textos ficarem fáceis de ler */
    --abnt-bg-azul: #ebf4ff;
    --abnt-bg-vermelho: #fef2f2;
    --abnt-bg-verde: #f0fdf4;
    --abnt-bg-amarelo: #fefce8;
  }

  /* ':global(body)' aplica essas regras para o site inteiro, removendo margens padrão do navegador */
  :global(body) {
    margin: 0;
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif; /* Fonte séria e profissional */
    background-color: #ffffff;
    color: #333;
    line-height: 1.6; /* Espaçamento entre as linhas do texto */
  }

  /* Classes utilitárias: atalhos rápidos para aplicar as cores nos textos e bordas */
  .text-azul {
    color: var(--abnt-azul) !important;
  }
  .text-vermelho {
    color: var(--abnt-vermelho) !important;
  }
  .text-verde {
    color: var(--abnt-verde) !important;
  }
  .text-amarelo {
    color: var(--abnt-amarelo) !important;
  }

  .border-azul {
    border-bottom: 5px solid var(--abnt-azul);
  }
  .border-vermelho {
    border-bottom: 5px solid var(--abnt-vermelho);
  }
  .border-verde {
    border-bottom: 5px solid var(--abnt-verde);
  }
  .border-amarelo {
    border-bottom: 5px solid var(--abnt-amarelo);
  }

  /* --- HERO SCROLL (A área da primeira imagem da cidade) --- */
  .hero-section {
    position: relative;
    /* Aumentado de 150vh para 250vh. Isso empurra o conteúdo de texto muito mais para baixo, 
       obrigando o usuário a rolar mais vezes a bolinha do mouse antes de ver o resto do site. */
    height: 250vh;
  }

  /* A 'city-layer' fica grudada (fixed) no fundo enquanto o resto do site sobe por cima dela */
  .city-layer {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    z-index: -1; /* Joga a imagem para trás de todos os outros elementos */
  }

  .clean-city {
    background: url("/cidade-limpa.png") center/cover no-repeat;
    background-color: var(
      --abnt-verde
    ); /* Cor de fundo caso a imagem falhe ao carregar */
  }

  .polluted-city {
    background: url("/cidade-poluida.png") center/cover no-repeat;
    background-color: #57534e;
    transition: opacity 0.1s ease-out; /* Deixa o escurecimento suave */
  }

  /* Texto animado que quica pedindo para a pessoa rolar a tela */
  .scroll-instruction {
    position: absolute;
    bottom: 20px;
    width: 100%;
    text-align: center;
    font-weight: bold;
    color: white;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.8);
    animation: bounce 2s infinite; /* Chama a animação 'bounce' logo abaixo */
  }

  /* Animação que faz o texto pular (translateY move ele no eixo Y/Vertical) */
  @keyframes bounce {
    0%,
    100% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(-10px);
    }
  }

  /* --- SEÇÕES GERAIS --- */
  /* Regras compartilhadas por todas as sessões brancas/coloridas do site */
  .content-section,
  .impacts-section,
  .lifecycle-section,
  .three-rs-section,
  .game-section {
    position: relative;
    padding: 4rem 2rem; /* Espaço interno para não grudar nas bordas */
    text-align: center;
    z-index: 1; /* Garante que ficarão acima da imagem de fundo fixa */
  }

  h2 {
    font-size: 2.5rem;
    margin-bottom: 1rem;
    font-weight: 700;
  }

  p {
    font-size: 1.1rem;
    max-width: 800px; /* Limita a largura do texto para não ficar ruim de ler em telas muito grandes */
    margin: 0 auto;
  }

  /* --- CONSCIENTIZAÇÃO (TEMA AZUL) --- */
  .content-section {
    background: var(--abnt-bg-azul);
    border-radius: 40px 40px 0 0; /* Arredonda só os cantos superiores */
    box-shadow: 0 -10px 20px rgba(0, 0, 0, 0.1); /* Dá uma sombrinha em cima para parecer que está flutuando sobre a imagem */
  }

  .content-section h2 {
    color: var(--abnt-azul);
  }

  /* Container flexível para os cards ficarem lado a lado (e caírem para baixo no celular) */
  .info-cards {
    display: flex;
    justify-content: center;
    gap: 2rem; /* Espaço entre os cards */
    margin-top: 3rem;
    flex-wrap: wrap; /* Se não couber, joga para a linha de baixo */
  }

  .card {
    background: white;
    border: 3px dashed;
    padding: 1.5rem;
    border-radius: 12px;
    width: 300px;
    transition: transform 0.3s; /* Deixa o aumento de tamanho (hover) suave */
  }

  .card-azul {
    border-color: var(--abnt-azul);
  }
  .card-vermelho {
    border-color: var(--abnt-vermelho);
  }
  .card-verde {
    border-color: var(--abnt-verde);
  }

  /* Pseudo-classe ':hover' faz algo quando o mouse passa por cima (neste caso, aumenta 2%) */
  .card:hover {
    transform: scale(1.02);
  }

  /* --- IMPACTOS NAS CIDADES (BRANCO/NEUTRO) --- */
  .impacts-section {
    background: #ffffff;
  }

  .impacts-section h2 {
    color: #333;
  }

  .impact-container {
    max-width: 1000px;
    margin: 3rem auto 0;
  }

  .impact-row {
    display: flex;
    flex-direction: column; /* Coloca um impacto debaixo do outro */
    gap: 1.5rem;
  }

  .impact-text {
    background: #f8fafc;
    padding: 2rem;
    border-radius: 8px;
    text-align: left;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  }

  .impact-text h3 {
    margin-top: 0;
    font-size: 1.4rem;
  }

  .impact-text p {
    margin: 0;
    max-width: 100%;
    color: #475569;
  }

  /* --- CICLO DOS MATERIAIS (FUNDO CLARO) --- */
  .lifecycle-section {
    background: #f8fafc;
  }

  .lifecycle-section h2 {
    color: #333;
  }

  /* Usa CSS Grid para criar uma grade automática de cards */
  .lifecycle-container {
    display: grid;
    grid-template-columns: repeat(
      auto-fit,
      minmax(280px, 1fr)
    ); /* Cria colunas que se adaptam ao tamanho da tela */
    gap: 2rem;
    max-width: 1200px;
    margin: 3rem auto 0;
    padding: 0 1rem;
  }

  .lifecycle-card {
    background: white;
    padding: 2rem;
    border-radius: 12px;
    text-align: left;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
    transition: transform 0.3s ease;
  }

  /* No hover, faz o card flutuar levemente para cima (eixo Y negativo) */
  .lifecycle-card:hover {
    transform: translateY(-5px);
  }

  .lifecycle-card h3 {
    margin-top: 0;
    font-size: 1.4rem;
    border-bottom: 1px solid #e2e8f0;
    padding-bottom: 0.5rem;
    margin-bottom: 1rem;
  }

  .lifecycle-card p {
    font-size: 1rem;
    margin: 0 0 1rem 0;
    color: #475569;
    line-height: 1.5;
  }

  /* --- OS 3 R's (TEMA AMARELO) --- */
  .three-rs-section {
    background: var(--abnt-bg-amarelo);
  }

  .three-rs-section h2 {
    color: var(--abnt-amarelo);
    text-shadow: 1px 1px 0px rgba(0, 0, 0, 0.1);
  }

  .rs-container {
    display: flex;
    justify-content: center;
    gap: 2rem;
    margin-top: 3rem;
    flex-wrap: wrap;
  }

  .r-card {
    background: white;
    padding: 2rem 1.5rem;
    border-radius: 12px;
    width: 280px;
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
  }

  .r-card:hover {
    transform: translateY(-5px);
  }

  .r-card h3 {
    font-size: 1.5rem;
    margin: 0 0 1rem 0;
  }

  .r-card p {
    font-size: 1rem;
    color: #4b5563;
  }

  /* --- JOGO (TEMA VERDE) --- */
  .game-section {
    background: var(--abnt-bg-verde);
    padding-bottom: 8rem;
  }

  .game-section h2 {
    color: var(--abnt-verde);
  }

  /* Caixa onde ficam os lixos bagunçados */
  .trash-area {
    min-height: 120px;
    border: 3px dashed; /* Borda pontilhada */
    border-radius: 12px;
    margin: 2rem auto;
    max-width: 800px;
    display: flex;
    gap: 1rem;
    justify-content: center;
    align-items: center;
    padding: 1.5rem;
    flex-wrap: wrap;
    background: white;
  }

  /* O lixo propriamente dito que pode ser arrastado */
  .trash-item {
    background: white;
    border: 2px solid #e2e8f0;
    padding: 1.5rem 1rem;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    cursor: grab; /* Muda o cursor do mouse para a "mãozinha aberta" */
    display: flex;
    flex-direction: column;
    align-items: center;
    user-select: none; /* Impede que o usuário selecione o texto acidentalmente ao tentar arrastar */
    transition: transform 0.2s;
    font-weight: bold;
  }

  /* ':active' é ativado no momento em que a pessoa clica e segura o mouse */
  .trash-item:active {
    cursor: grabbing; /* Muda para a "mãozinha fechada" */
    transform: scale(
      0.95
    ); /* Dá um efeitinho de afundamento (diminui tamanho pra 95%) */
  }

  .empty-trash-message {
    font-size: 1.3rem;
    color: var(--abnt-verde);
    font-weight: bold;
  }

  /* A área onde ficam as lixeiras maiores */
  .bins-area {
    display: flex;
    justify-content: center;
    gap: 2rem;
    margin-top: 3rem;
    flex-wrap: wrap;
  }

  /* Desenho estrutural de cada lixeira usando CSS */
  .bin {
    width: 120px;
    height: 160px;
    border: 4px solid;
    border-radius: 8px 8px 16px 16px; /* Arredonda menos em cima e mais embaixo */
    display: flex;
    flex-direction: column;
    background: white;
    transition: transform 0.2s;
  }

  .bin:hover {
    transform: scale(1.05);
  }

  /* Desenho da tampa da lixeira */
  .bin-cap {
    height: 30px;
    width: 110%; /* Passa um pouco do tamanho do corpo da lixeira (como uma tampa real) */
    margin-left: -5%; /* Centraliza esse excesso */
    border-radius: 6px 6px 0 0;
    margin-bottom: auto;
  }

  .bin-body {
    padding: 1rem 0;
    font-size: 0.9rem;
    font-weight: bold;
    min-height: 20px;
  }

  /* --- MODAL (JANELA SOBREPOSTA) --- */
  /* Camada escura que fica atrás do modal cobrindo a tela toda */
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw; /* 100% da largura da janela */
    height: 100vh; /* 100% da altura da janela */
    background: rgba(0, 0, 0, 0.7); /* Preto com 70% de opacidade */
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000; /* Garante que fique absoluto por cima de TUDO no site */
  }

  /* O quadrado branco da mensagem em si */
  .modal-content {
    background: white;
    padding: 3rem;
    border-radius: 16px;
    text-align: center;
    max-width: 500px;
    box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3);
    animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); /* Faz o modal "pular" e quicar quando aparece */
  }

  /* Classes dinâmicas para colorir a borda superior do modal dependendo se acertou ou errou */
  .modal-content.success {
    border-top: 8px solid var(--abnt-verde);
  }
  .modal-content.error {
    border-top: 8px solid var(--abnt-vermelho);
  }
  .modal-content.finish {
    border-top: 8px solid var(--abnt-azul);
    background: var(--abnt-bg-azul);
  }

  /* Estilização do botão de "Continuar" */
  .modal-btn {
    margin-top: 1.5rem;
    padding: 0.75rem 2rem;
    font-size: 1.1rem;
    font-weight: bold;
    background: var(--abnt-azul);
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background 0.3s;
  }

  .modal-btn:hover {
    background: #003d99;
  }

  /* Configuração do "quique" da animação do modal */
  @keyframes popIn {
    0% {
      transform: scale(0.8);
      opacity: 0;
    }
    100% {
      transform: scale(1);
      opacity: 1;
    }
  }
</style>
