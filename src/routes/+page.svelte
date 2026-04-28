<script lang="ts">
  import { onMount } from "svelte";

  // --- CONTROLE DE SCROLL ---
  let scrollY = 0;
  let innerHeight = 0;

  $: pollutionOpacity = Math.min(scrollY / (innerHeight * 0.8), 1);
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
          o descarte em uma experiêcia de aprendizado ativa.
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

    <section class="cta-game-section">
    <div class="cta-container">
      <h2>Pronto para testar seus conhecimentos?</h2>
      <p>Divirta-se e aprenda a reciclar com o nosso simulador interativo!</p>
      <a href="https://intercurso.vercel.app" target="_blank" class="game-button">
        JOGAR AGORA
      </a>
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

  /* --- HERO --- */
  .hero-section {
    position: relative;
    height: 200vh;
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
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-15px); }
  }

  /* --- SEÇÕES GERAIS --- */
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

  .content-section {
    background: var(--abnt-bg-azul);
    border-radius: 40px 40px 0 0;
    margin-top: -2px;
  }

  .info-cards, .rs-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
    margin-top: 3rem;
    max-width: 1100px;
    margin-left: auto;
    margin-right: auto;
  }

  .card, .r-card {
    background: white;
    padding: 2rem;
    border-radius: 15px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
    border-bottom: 5px solid transparent;
  }

  /* --- ESTILO DA NOVA SEÇÃO DO JOGO --- */
  .cta-game-section {
    background-color: var(--abnt-bg-verde);
    padding: 5rem 1.5rem;
  }

  .cta-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 3rem;
    background: white;
    border-radius: 25px;
    border: 4px dashed var(--abnt-verde);
    box-shadow: 0 15px 30px rgba(0,0,0,0.1);
  }

  .game-button {
    display: inline-block;
    margin-top: 2rem;
    padding: 1.2rem 2.5rem;
    background-color: var(--abnt-verde);
    color: white;
    text-decoration: none;
    font-weight: bold;
    font-size: 1.2rem;
    border-radius: 50px;
    transition: transform 0.2s, background-color 0.2s;
  }

  .game-button:hover {
    background-color: #128a3e;
    transform: scale(1.05);
  }

  /* --- IMPACTOS --- */
  .impacts-section { background: #ffffff; }
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

  /* --- UTILITÁRIOS DE COR --- */
  .text-azul { color: var(--abnt-azul); }
  .text-vermelho { color: var(--abnt-vermelho); }
  .text-verde { color: var(--abnt-verde); }
  .text-amarelo { color: var(--abnt-amarelo); }
  .text-repensar { color: var(--repensar-color); }
  .text-recusar { color: var(--recusar-color); }
  .border-azul { border-color: var(--abnt-azul); }
  .border-vermelho { border-color: var(--abnt-vermelho); }
  .border-verde { border-color: var(--abnt-verde); }
  .border-amarelo { border-color: var(--abnt-amarelo); }
  .border-repensar { border-color: var(--repensar-color); }
  .border-recusar { border-color: var(--recusar-color); }

  @media (max-width: 480px) {
    .cta-container { padding: 1.5rem; }
    .hero-section { height: 160vh; }
  }
</style>