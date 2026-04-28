<script lang="ts">
  import { onMount } from "svelte";

  // --- CONTROLE DE SCROLL ---
  let scrollY = 0;
  let innerHeight = 0; // Pegamos a altura da tela do usuário

  // MELHORIA NA TRANSIÇÃO:
  // Em vez de dividir por um valor fixo (800), usamos uma proporção da altura da tela.
  // Isso garante que tanto no PC quanto no Celular, a transição seja proporcional ao que o usuário vê.
  $: pollutionOpacity = Math.min(scrollY / (innerHeight * 0.8), 1);

  // --- DADOS DO JOGO ---
  type TrashType = "plastico" | "papel" | "vidro" | "metal";

  interface TrashItem {
    id: number;
    name: string;
    type: TrashType;
    impactMessage: string;
  }

  interface Bin {
    type: TrashType;
    color: string;
    name: string;
  }

  const bins: Bin[] = [
    { type: "papel", color: "#0052cc", name: "Papel (Azul)" },
    { type: "plastico", color: "#dc2626", name: "Plástico (Vermelho)" },
    { type: "vidro", color: "#16a34a", name: "Vidro (Verde)" },
    { type: "metal", color: "#ca8a04", name: "Metal (Amarelo)" },
  ];

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

  let trashItems: TrashItem[] = [...initialTrashItems];
  let currentRound = 1;

  // --- ESTADO DO MODAL ---
  let modalVisible = false;
  let modalTitle = "";
  let modalMessage = "";
  let modalType: "success" | "error" | "finish" = "success";

  // --- LÓGICA DO JOGO ---
  let draggedItem: TrashItem | null = null;

  function handleDragStart(item: TrashItem) {
    draggedItem = item;
  }

  function handleDrop(binType: TrashType) {
    if (!draggedItem) return;

    if (draggedItem.type === binType) {
      showModal("Parabéns!", draggedItem.impactMessage, "success");
      trashItems = trashItems.filter((item) => item.id !== draggedItem?.id);

      if (trashItems.length === 0) {
        if (currentRound === 1) {
          setTimeout(() => {
            showModal(
              "Nível 2 Desbloqueado!",
              "Agora vamos testar sua memória. As lixeiras perderam os textos, use apenas as cores para acertar!",
              "success",
            );
            currentRound = 2;
            trashItems = [...initialTrashItems];
          }, 1500);
        } else {
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
      showModal(
        "Ops! Lixeira Errada",
        `Esse item não vai nessa lixeira. Tente novamente!`,
        "error",
      );
    }
    draggedItem = null;
  }

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

  function closeModal() {
    modalVisible = false;
  }
</script>

<svelte:window bind:scrollY bind:innerHeight />

<main>
  <section class="hero-section">
    <div class="city-layer clean-city"></div>
    <div
      class="city-layer polluted-city"
      style="opacity: {pollutionOpacity};"
    ></div>
    <div class="scroll-instruction">
      <span>Arraste para baixo</span>
      <br />
      Para onde vai o lixo quando ele sai da sua frente?
    </div>
  </section>

  <section class="content-section">
    <h2>Projeto Re-ciclo</h2>
    <p>
      O Problema: Cada pessoa gera em torno de 25 toneladas de lixo durante a
      vida. O grande desafio da nossa sociedade é que, se não vemos o lixo, não
      nos importamos com ele.
    </p>

    <div class="info-cards">
      <div class="card card-azul">
        <h3 class="text-azul">A Solução</h3>
        <p>
          Desenvolvemos uma intervenção pedagógica gamificada. O jogo transforma
          o descarte em uma experiência de aprendizado ativa.
        </p>
      </div>
      <div class="card card-vermelho">
        <h3 class="text-vermelho">Nosso Objetivo</h3>
        <p>
          Acreditamos que a solução não é apenas "limpar", mas sim "educar para
          não sujar" através da visibilidade do processo.
        </p>
      </div>
      <div class="card card-verde">
        <h3 class="text-verde">Público-Alvo</h3>
        <p>
          A intervenção é voltada para crianças do ensino fundamental 2, mas o
          impacto se estende aos pais e à escola de forma geral.
        </p>
      </div>
    </div>
  </section>

  <section class="impacts-section">
    <h2>ODS e Impacto</h2>
    <p>
      Desenvolvido por uma equipe dedicada ao desenvolvimento sustentável
      global.
    </p>

    <div class="impact-container">
      <div class="impact-row">
        <div class="impact-text border-azul">
          <h3 class="text-azul">ODS 4</h3>
          <p>
            Educação de Qualidade. Através de nossa metodologia, aliamos o
            ensino ao entretenimento consciente.
          </p>
        </div>
        <div class="impact-text border-verde">
          <h3 class="text-verde">ODS 11</h3>
          <p>
            Cidades e Comunidades Sustentáveis. Reforçando a importância da
            correta gestão de resíduos urbanos.
          </p>
        </div>
        <div class="impact-text border-amarelo">
          <h3 class="text-amarelo">ODS 12</h3>
          <p>
            Consumo e Produção Responsáveis. Incentivando a reflexão sobre as 25
            toneladas geradas por indivíduo.
          </p>
        </div>
      </div>
    </div>
  </section>

  <section class="five-rs-section">
    <h2>A Regra dos 5 R's</h2>
    <div class="rs-container">
      <div class="r-card border-repensar">
        <h3 class="text-repensar">1. Repensar</h3>
        <p>Cada compra deve ser consciente. Eu realmente preciso disso?</p>
      </div>
      <div class="r-card border-recusar">
        <h3 class="text-recusar">2. Recusar</h3>
        <p>Diga não a produtos que prejudicam o meio ambiente.</p>
      </div>
      <div class="r-card border-vermelho">
        <h3 class="text-vermelho">3. Reduzir</h3>
        <p>Diminuir o consumo desnecessário no dia a dia.</p>
      </div>
      <div class="r-card border-amarelo">
        <h3 class="text-amarelo">4. Reutilizar</h3>
        <p>Dar novas utilidades a objetos antes de jogá-los fora.</p>
      </div>
      <div class="r-card border-verde">
        <h3 class="text-verde">5. Reciclar</h3>
        <p>Separar corretamente para que o material vire um novo produto.</p>
      </div>
    </div>
  </section>

  <section class="map-section">
    <h2>Ecopontos em Fortaleza</h2>
    <div class="map-container">
      <iframe
        title="Mapa de Ecopontos de Fortaleza"
        src="https://www.google.com/maps/embed?pb=!1m16!1m12!1m3!1d127415.42621021469!2d-38.583168019623774!3d-3.7631336440590833!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!2m1!1secopontos%20fortaleza!5e0!3m2!1spt-BR!2sbr!4v1715870000000!5m2!1spt-BR!2sbr"
        width="100%"
        height="450"
        style="border:0; border-radius: 15px;"
        allowfullscreen
        loading="lazy"
        referrerpolicy="no-referrer-when-downgrade"
      >
      </iframe>
    </div>
    <div class="map-info">
      <div class="info-item">
        <strong>O que levar:</strong> Papel, plástico, vidro, metal, entulhos (até
        2m³), móveis e podas.
      </div>
      <div class="info-item">
        <strong>Benefício:</strong> Ganhe bônus na conta de luz ou no Bilhete Único.
      </div>
    </div>
  </section>

  <section class="game-section">
    <h2>O Jogo da Reciclagem</h2>
    <div class="trash-area">
      {#each trashItems as item (item.id)}
        <div
          class="trash-item"
          draggable="true"
          on:dragstart={() => handleDragStart(item)}
        >
          <span>{item.name}</span>
        </div>
      {/each}
    </div>

    <div class="bins-area">
      {#each bins as bin}
        <div
          class="bin"
          style="border-color: {bin.color};"
          on:dragover|preventDefault
          on:drop={() => handleDrop(bin.type)}
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
    <div class="modal-overlay" on:click={closeModal} role="button" tabindex="0">
      <div
        class="modal-content {modalType}"
        on:click|stopPropagation
        role="dialog"
        tabindex="0"
      >
        <h2>{modalTitle}</h2>
        <p>{modalMessage}</p>
        <button on:click={closeModal} class="modal-btn">Continuar</button>
      </div>
    </div>
  {/if}
</main>

<style>
  :root {
    --abnt-azul: #0052cc;
    --abnt-vermelho: #dc2626;
    --abnt-verde: #16a34a;
    --abnt-amarelo: #ca8a04;
    --repensar-color: #7c3aed;
    --recusar-color: #4b5563;
    --abnt-bg-azul: #ebf4ff;
    --abnt-bg-verde: #f0fdf4;
    --bg-repensar: #f5f3ff;
  }

  :global(body) {
    margin: 0;
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
    background-color: #ffffff;
    color: #333;
    line-height: 1.6;
    overflow-x: hidden;
    width: 100%;
  }

  /* --- HERO (MELHORIA NA TRANSIÇÃO MOBILE) --- */
  .hero-section {
    position: relative;
    height: 200vh; /* Aumentado para dar mais espaço de scroll */
    width: 100%;
  }

  .city-layer {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    z-index: -1;
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;
    /* Suaviza a troca de opacidade */
    transition: opacity 0.1s linear;
  }

  .clean-city {
    background-image: url("/cidade-limpa.png");
    background-color: var(--abnt-verde);
  }

  .polluted-city {
    background-image: url("/cidade-poluida.png");
    background-color: #57534e;
  }

  .scroll-instruction {
    position: absolute;
    bottom: 15vh;
    width: 100%;
    padding: 0 1rem;
    box-sizing: border-box;
    text-align: center;
    font-size: clamp(1rem, 5vw, 1.3rem);
    font-weight: bold;
    color: white;
    text-shadow: 2px 2px 6px rgba(0, 0, 0, 1);
    animation: bounce 2s infinite;
  }

  @keyframes bounce {
    0%,
    100% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(-15px);
    }
  }

  /* --- SEÇÕES RESPONSIVAS --- */
  section {
    padding: 4rem 1.5rem;
    text-align: center;
    position: relative;
    z-index: 1;
    background-color: #ffffff;
  }

  h2 {
    font-size: clamp(1.8rem, 8vw, 2.5rem);
    margin-bottom: 1.5rem;
  }
  p {
    max-width: 800px;
    margin: 0 auto;
    font-size: 1.05rem;
  }

  .content-section {
    background: var(--abnt-bg-azul);
    border-radius: 40px 40px 0 0;
    margin-top: -2px;
  }

  .info-cards,
  .rs-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
    margin-top: 3rem;
    max-width: 1100px;
    margin-left: auto;
    margin-right: auto;
  }

  .card,
  .r-card {
    background: white;
    padding: 2rem;
    border-radius: 15px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
    border-bottom: 5px solid transparent;
  }

  /* --- IMPACTOS (FUNDO BRANCO) --- */
  .impacts-section {
    background: #ffffff;
  }
  .impact-row {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    max-width: 800px;
    margin: 2rem auto;
  }
  .impact-text {
    background: #f8fafc;
    padding: 1.5rem;
    border-radius: 12px;
    text-align: left;
  }

  /* --- MAPA --- */
  .map-container {
    width: 100%;
    max-width: 1000px;
    margin: 2rem auto;
    border-radius: 15px;
    overflow: hidden;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  }
  .map-info {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1rem;
    max-width: 1000px;
    margin: 0 auto;
  }
  .info-item {
    background: #f8fafc;
    padding: 1rem;
    border-radius: 10px;
    border-left: 5px solid var(--abnt-verde);
    text-align: left;
  }

  /* --- JOGO (RESPONSIVO) --- */
  .game-section {
    background: var(--abnt-bg-verde);
    padding-bottom: 5rem;
  }
  .trash-area {
    min-height: 100px;
    border: 2px dashed var(--abnt-verde);
    border-radius: 15px;
    background: white;
    max-width: 600px;
    margin: 2rem auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 0.8rem;
    padding: 1rem;
  }
  .trash-item {
    background: white;
    padding: 0.7rem 1rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    cursor: grab;
    font-weight: bold;
  }

  .bins-area {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 1rem;
    max-width: 400px;
    margin: 0 auto;
  }
  @media (min-width: 768px) {
    .bins-area {
      grid-template-columns: repeat(4, 1fr);
      max-width: 800px;
    }
  }

  .bin {
    width: 100%;
    height: 140px;
    border: 3px solid;
    border-radius: 10px;
    background: white;
    display: flex;
    flex-direction: column;
  }
  .bin-cap {
    height: 25px;
    width: 100%;
    border-radius: 5px 5px 0 0;
  }
  .bin-body {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.8rem;
  }

  /* --- MODAL --- */
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.8);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 9999;
    padding: 1rem;
    box-sizing: border-box;
  }
  .modal-content {
    background: white;
    padding: 2rem;
    border-radius: 20px;
    width: 100%;
    max-width: 400px;
    border-top: 10px solid;
  }

  /* --- UTILITÁRIOS --- */
  .text-azul {
    color: var(--abnt-azul);
  }
  .text-vermelho {
    color: var(--abnt-vermelho);
  }
  .text-verde {
    color: var(--abnt-verde);
  }
  .text-amarelo {
    color: var(--abnt-amarelo);
  }
  .text-repensar {
    color: var(--repensar-color);
  }
  .text-recusar {
    color: var(--recusar-color);
  }
  .border-azul {
    border-color: var(--abnt-azul);
  }
  .border-vermelho {
    border-color: var(--abnt-vermelho);
  }
  .border-verde {
    border-color: var(--abnt-verde);
  }
  .border-amarelo {
    border-color: var(--abnt-amarelo);
  }
  .border-repensar {
    border-color: var(--repensar-color);
  }
  .border-recusar {
    border-color: var(--recusar-color);
  }

  /* --- AJUSTES ESPECÍFICOS MOBILE --- */
  @media (max-width: 480px) {
    .hero-section {
      height: 160vh;
    }
    .scroll-instruction {
      bottom: 10vh;
    }
    .info-cards,
    .rs-container {
      grid-template-columns: 1fr;
    }
  }
</style>
