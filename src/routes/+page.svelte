<script lang="ts">
  import { dev } from "$app/environment";
  import { injectAnalytics } from "@vercel/analytics/sveltekit";

  injectAnalytics({ mode: dev ? "development" : "production" });

  // --- CONTROLE DE SCROLL ---
  let scrollY = 0;
  let innerHeight = 0;

  // Transição proporcional baseada na tela do usuário
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
      Para onde vai o lixo quando você joga na lixeira da facul?
    </div>
  </section>

  <section class="content-section">
    <h2>Nosso Lixo e a Economia Circular</h2>
    <p>
      Você sabia que o Brasil gera cerca de <strong
        >82 milhões de toneladas</strong
      > de lixo por ano? O nosso grande desafio atual é que a gente se acostumou
      a extrair, usar e simplesmente jogar as coisas fora. Mas precisamos mudar isso!
    </p>

    <div class="info-cards">
      <div class="card card-azul">
        <h3 class="text-azul">Para onde vai tudo isso?</h3>
        <p>
          Infelizmente, quase 30 milhões de toneladas desse lixo ainda vão parar
          em lixões a céu aberto todos os anos. Isso polui nossa água, o solo e
          solta gases que prejudicam o clima.
        </p>
      </div>
      <div class="card card-vermelho">
        <h3 class="text-vermelho">A ilusão do "Jogar Fora"</h3>
        <p>
          Já parou para pensar que não existe o "fora"? Tudo o que a gente
          descarta continua no nosso planeta. Uma embalagem de plástico pode
          ficar por séculos ocupando espaço na natureza.
        </p>
      </div>
      <div class="card card-verde">
        <h3 class="text-verde">Economia Circular</h3>
        <p>
          A ideia da Economia Circular é incrível: o que é "lixo" para um
          processo, vira matéria-prima nova para outro. Mas para essa mágica
          acontecer, a gente precisa separar o lixo direitinho!
        </p>
      </div>
    </div>
  </section>

  <section class="impacts-section">
    <h2>A Agenda 2030 e a Nossa Parte</h2>
    <p>
      Cuidar do nosso lixo tem tudo a ver com os Objetivos de Desenvolvimento
      Sustentável (ODS) da ONU. E a universidade é o lugar perfeito para a gente
      começar essa mudança e espalhar a ideia!
    </p>

    <div class="impact-container">
      <div class="impact-row">
        <div class="impact-text border-azul">
          <h3 class="text-azul">ODS 4: Educação de Qualidade</h3>
          <p>
            Aprender a cuidar do planeta também faz parte de uma educação de
            verdade. Queremos formar não só bons profissionais, mas cidadãos que
            pensem no futuro do meio ambiente.
          </p>
        </div>
        <div class="impact-text border-verde">
          <h3 class="text-verde">ODS 11: Cidades e Comunidades Sustentáveis</h3>
          <p>
            Tudo começa no campus! Separar o lixo ajuda a não sobrecarregar
            nossa cidade, evitando problemas graves como enchentes causadas por
            bueiros entupidos e até doenças.
          </p>
        </div>
        <div class="impact-text border-amarelo">
          <h3 class="text-amarelo">ODS 12: Consumo e Produção Responsáveis</h3>
          <p>
            Precisamos pensar bem antes de comprar e descartar. Diminuir a
            quantidade de coisas que consumimos e apoiar a reciclagem local faz
            uma diferença gigante no mundo.
          </p>
        </div>
      </div>
    </div>
  </section>

  <section class="five-rs-section">
    <h2>A Regra dos 5 R's na Prática</h2>
    <div class="rs-container">
      <div class="r-card border-repensar">
        <h3 class="text-repensar">1. Repensar</h3>
        <p>
          Eu realmente preciso comprar isso? Repense seus hábitos, o que você
          consome e de quais empresas você compra.
        </p>
      </div>
      <div class="r-card border-recusar">
        <h3 class="text-recusar">2. Recusar</h3>
        <p>
          Diga "não" a plásticos de uso único e a marcas que fazem <strong
            >greenwashing</strong
          > (aquela prática enganosa onde a empresa finge ser sustentável só no marketing
          para vender mais, mas não ajuda o planeta de verdade).
        </p>
      </div>
      <div class="r-card border-vermelho">
        <h3 class="text-vermelho">3. Reduzir</h3>
        <p>
          Diminua a quantidade de lixo no dia a dia. Use garrafinhas e copos
          retornáveis e evite imprimir papeis que você pode ler na tela.
        </p>
      </div>
      <div class="r-card border-amarelo">
        <h3 class="text-amarelo">4. Reutilizar</h3>
        <p>
          Dê uma nova vida às coisas! Conserte, doe, compre de brechós ou use a
          criatividade para transformar o que iria para a lixeira.
        </p>
      </div>
      <div class="r-card border-verde">
        <h3 class="text-verde">5. Reciclar</h3>
        <p>
          Limpe um pouquinho a embalagem e jogue na lixeira da cor certa
          (Plástico no vermelho, Papel no azul...). Assim, o material volta para
          a fábrica e vira algo novo!
        </p>
      </div>
    </div>
  </section>

  <section class="special-waste-section">
    <h2>E os lixos mais "delicados"?</h2>
    <p>
      Nem tudo pode ir para a lixeira comum ou para o Ecoponto de papel e
      plástico. Alguns materiais são super perigosos para a natureza se
      descartados do jeito errado. Veja como fazer:
    </p>

    <div class="special-waste-container">
      <div class="waste-card border-laranja">
        <h3 class="text-laranja">Pilhas e Baterias</h3>
        <p>
          <strong>O perigo:</strong> Elas têm metais pesados (como chumbo e mercúrio)
          que vazam e contaminam o solo e a água de forma irreversível.
        </p>
        <p>
          <strong>Como descartar:</strong> Guarde em um pote seco e leve até pontos
          de coleta específicos. Supermercados, farmácias e lojas de eletrônicos
          geralmente têm caixas coletoras para a cor laranja.
        </p>
      </div>

      <div class="waste-card border-roxo">
        <h3 class="text-roxo">Lixo Eletrônico</h3>
        <p>
          <strong>O perigo:</strong> Celulares, cabos e computadores velhos demoram
          séculos para sumir e soltam substâncias tóxicas. Além disso, jogá-los fora
          é desperdiçar metais valiosos (como ouro e cobre) que poderiam ser reaproveitados.
        </p>
        <p>
          <strong>Como descartar:</strong> Procure as lixeiras roxas ou mutirões
          de coleta de lixo eletrônico na sua cidade ou universidade. Várias ONGs
          e empresas recolhem esse material de graça.
        </p>
      </div>

      <div class="waste-card border-marrom">
        <h3 class="text-marrom">Óleo de Cozinha Usado</h3>
        <p>
          <strong>O perigo:</strong> Nunca jogue na pia! Apenas 1 litro de óleo pode
          contaminar até 25 mil litros de água, matando peixes e entupindo todo o
          encanamento da rua.
        </p>
        <p>
          <strong>Como descartar:</strong> Espere esfriar, coloque em uma garrafa
          PET bem fechada e entregue em Ecopontos ou para pessoas/empresas que fazem
          sabão ecológico.
        </p>
      </div>
    </div>
  </section>

  <section class="diy-section">
    <h2>Mão na Massa: Upcycling (DIY)</h2>
    <p>
      Você também pode fazer a sua parte em casa! O "Upcycling" é a arte de dar
      um novo uso criativo e prático para algo que iria para o lixo. Dá uma
      olhada nessas ideias super fáceis que separamos:
    </p>

    <div class="video-container">
      <div class="video-card border-vermelho">
        <h3 class="text-vermelho">Plástico: Vaso Auto-irrigável</h3>
        <div class="iframe-wrapper">
          <iframe
            src="https://www.youtube.com/embed/9LMItr89MpU"
            title="Como fazer vaso autoirrigável com garrafa PET"
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen
          >
          </iframe>
        </div>
        <p>
          Sabe aquela garrafa PET do refri? Ela pode virar uma hortinha
          inteligente para o seu quarto ou varanda, economizando água e espaço.
        </p>
      </div>

      <div class="video-card border-azul">
        <h3 class="text-azul">Papel: Papel Reciclado Caseiro</h3>
        <div class="iframe-wrapper">
          <iframe
            src="https://www.youtube.com/embed/N8iOrSae9Tw"
            title="Como fazer papel reciclado caseiro"
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen
          >
          </iframe>
        </div>
        <p>
          Sabe aquelas apostilas antigas ou resumos do semestre passado? Que tal
          bater no liquidificador e fazer papéis novos e super estilosos para
          resumos e cartões?
        </p>
      </div>

      <div class="video-card border-verde">
        <h3 class="text-verde">Vidro: Decoração Criativa</h3>
        <div class="iframe-wrapper">
          <iframe
            src="https://www.youtube.com/embed/pxTspV5JVUA"
            title="Ideias de reciclagem com potes de vidro"
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen
          >
          </iframe>
        </div>
        <p>
          O vidro é ótimo porque não passa cheiro nem gosto. Você pode limpar
          aqueles potes de maionese para guardar mantimentos, temperos ou até
          fazer luminárias iradas.
        </p>
      </div>
    </div>
  </section>

  <section class="map-section">
    <h2>Faça a sua Parte: Ecopontos em Fortaleza</h2>
    <p style="margin-bottom: 2rem;">
      Bora colocar a mão na massa? Encontre o Ecoponto mais perto de você para
      levar seus recicláveis!
    </p>
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
        <strong>O que eles aceitam:</strong> Plástico, papel, papelão, vidro, metais,
        entulho de obras pequenas (até 2m³), restos de poda de árvores e até móveis
        velhos.
      </div>
      <div class="info-item">
        <strong>Tem desconto!</strong> Sabia que o programa Recicla Fortaleza troca
        seu lixo reciclável por descontos na conta de luz (Enel) ou crédito no Bilhete
        Único? Vale muito a pena!
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
    --abnt-laranja: #ea580c; /* Pilhas e Baterias (Perigosos) */
    --abnt-roxo: #7c3aed; /* Lixo Radioativo/Eletrônico */
    --abnt-marrom: #854d0e; /* Orgânico/Óleo */
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
  /* --- HERO --- */
  .hero-section {
    position: relative;
    height: 200vh;
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
    0%,
    100% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(-15px);
    }
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
    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.1);
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
    transition:
      transform 0.2s,
      background-color 0.2s;
  }

  .game-button:hover {
    background-color: #128a3e;
    transform: scale(1.05);
  }

  /* --- IMPACTOS --- */
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

  /* --- LIXOS ESPECIAIS --- */
  .special-waste-section {
    background: #f8fafc;
  }
  .special-waste-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    max-width: 1100px;
    margin: 3rem auto 0;
  }
  .waste-card {
    background: white;
    padding: 1.5rem;
    border-radius: 15px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
    border-bottom: 5px solid transparent;
    text-align: center;
  }
  .waste-card h3 {
    margin-top: 0;
    font-size: 1.3rem;
    margin-bottom: 1rem;
  }
  .waste-card p {
    font-size: 1rem;
    color: #475569;
    margin-top: 1rem;
  }

  /* --- DIY E UPCYCLING SECTION --- */
  .diy-section {
    background: #ffffff;
  }
  .video-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    max-width: 1100px;
    margin: 3rem auto 0;
  }
  .video-card {
    background: white;
    padding: 1.5rem;
    border-radius: 15px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
    border-bottom: 5px solid transparent;
    text-align: left;
    display: flex;
    flex-direction: column;
  }
  .video-card h3 {
    margin-top: 0;
    font-size: 1.3rem;
    margin-bottom: 1rem;
  }
  .video-card p {
    font-size: 1rem;
    color: #475569;
    margin-top: 1rem;
  }
  .iframe-wrapper {
    position: relative;
    width: 100%;
    padding-bottom: 56.25%; /* Proporção 16:9 */
    height: 0;
    border-radius: 10px;
    overflow: hidden;
  }
  .iframe-wrapper iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border: 0;
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
  .text-laranja {
    color: var(--abnt-laranja);
  }
  .text-roxo {
    color: var(--abnt-roxo);
  }
  .text-marrom {
    color: var(--abnt-marrom);
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
  .border-laranja {
    border-color: var(--abnt-laranja);
  }
  .border-roxo {
    border-color: var(--abnt-roxo);
  }
  .border-marrom {
    border-color: var(--abnt-marrom);
  }
  .border-repensar {
    border-color: var(--repensar-color);
  }
  .border-recusar {
    border-color: var(--recusar-color);
  }

  @media (max-width: 480px) {
    .cta-container {
      padding: 1.5rem;
    }
    .hero-section {
      height: 160vh;
    }
    .scroll-instruction {
      bottom: 10vh;
    }
    .info-cards,
    .rs-container,
    .special-waste-container,
    .video-container {
      grid-template-columns: 1fr;
    }
  }
</style>
