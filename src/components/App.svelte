<script>
  // Write your JS here, or import other files
</script>

<main>
  <h1>Developing a LLM-Driven Multi-Agent Framework for Multimodal Translation</h1>

  <p class="authors">
    <a href="https://www.linkedin.com/in/feiyang-jiang/">Feiyang Jiang *</a>, <a href="#">Kristina Wu *</a>, 
    <a href="#">Nick Jumaoas *</a>, <a href="#">Hao Zhang</a>
    <br> University of California, San Diego
  </p>

  <p class="subtitle">2025 Data Science Senior Capstone</p>

  <div class="buttons">
    <a href="#" class="btn"> Paper</a>
    <a href="https://github.com/nljumaoas/dsc180b" class="btn"> Code</a>
    <a href="#" class="btn"> Poster</a>
    <a href="https://github.com/mantra-inc/open-mantra-dataset" class="btn"> Data</a>
  </div>

  <section class="content">
    <h2>Motivation</h2>
    <p>The increasing global demand for translated content, particularly in multimedia formats like manga, comic book, and literary works, has highlighted significant challenges in maintaining both efficiency and quality in translation processes. Traditional manual translation workflows, which rely heavily on human translators, editors, and typesetters, often struggle with scalability and consistency when handling large volumes of content that combines both textual and visual elements. This challenge becomes particularly acute in the context of manga translation, where cultural nuances, visual context, and textual accuracy must be carefully balanced by skilled professionals, leading to substantial time investments and increased production costs.</p>
    <p>Our goal is to combine recent advancements in multi-agent translation systems and context-aware multimodal translation, leveraging the increasing power of LLMs and VLMs to create a manga translation pipeline that has both convenience of machine translation and the quality of human translation. In doing so, we aim to demonstrate the potential of modular multi-agent frameworks to flexibly accommodate a wide variety of media, which we believe can facilitate the globalization of translated content at affordable costs, allowing for the enrichment of people all around the world.</p>
    
    <h2>Dataset</h2>
    <p>Our primary baseline dataset comes from OpenMantra, an automatic manga translation project that provides 214 manually translated and annotated manga pages for use as a benchmark for future research. </p>
    <p>Notably, although we aim to match the general format for evaluation purposes, our identified text fields have a much tighter fit to facilitate typesetting. Additionally, while the English translation of the pictured text is used to evaluate the overall accuracy of our translation framework, we expect differences due to subjective measures of fluency and reader perception.</p>

    <h2>Methods</h2>
    <p>Our innovated method pipeline involves three stages with the collaboration of agents... (more to come)</p>
    <img src="method_pipeline.png" alt="Method Pipeline Diagram" class="content-image">

    <h2>Evaluation</h2>
    <p>For evaluation method, we conducted ... (more to come with text explanation and equations)</p>

    <h2>Results</h2>
    <p>The final result has shown that ... (will include a short summary of the result and also a few plots and tables to illustrate)</p>
    <img src="table_metrics.png" alt="table of metrics evaluation" class="eval-image">
    <img src="human_eval.png" alt="human evaluation result" class="eval-image">

    <h2>Conclusion</h2>
    <p>Overall, our pipeline... (one short paragraph to summarize findings and suggestions for future work).</p>

    <h2>References</h2>
    <p>Motivation</p>
  </section>
  
</main>

<style>
  /* Write your CSS here */
  /* General body styles */
  body {
    font-family: 'Arial', sans-serif;
    background-color: #f4f4f9; /* Light background color */
    color: #333; /* Dark text for readability */
    margin: 0;
    padding: 0;
    justify-content: center; /* Horizontally center */
    align-items: center; /* Vertically center */
    height: 100vh; /* Full viewport height */
    background-image: linear-gradient(135deg, #6e7dff, #4d5eff); /* Soft gradient */
  }

  main {
    margin-left: auto;
    margin-right: auto;
    background: #fff; /* White background for the content */
    padding: 20px 40px;
    border-radius: 8px; /* Rounded corners */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Subtle shadow for depth */
    width: 100%; /* Make the width flexible */
    max-width: 1000px; /* Limit the width of the content */
    text-align: center; /* Center all text inside main */
  }

  h1 {
    font-size: 2.5rem;
    color: #333;
    margin-bottom: 20px;
    font-weight: bold;
  }

  .authors {
    font-size: 1rem;
    margin-top: 10px;
  }

  .authors a {
    color: #3070ff;
    text-decoration: none;
    font-weight: bold;
  }

  .subtitle {
    font-style: italic;
    color: #555;
    font-size: 0.9rem;
  }

  .buttons {
    margin-top: 20px;
  }

  .btn {
    display: inline-block;
    background: #333;
    color: white;
    padding: 10px 20px;
    margin: 5px;
    border-radius: 20px;
    text-decoration: none;
    font-size: 1rem;
  }

  .btn:hover {
    background: #555;
  }

  .content {
    text-align: left; /* Aligns the content section to the left */
    margin-top: 20px;
  }

  .content h2 {
    color: #80aaff;
    font-size: 1.5rem;
    font-weight: bold;
    margin-top: 20px;
  }

  .content p {
    font-size: 1rem;
    color: #444;
    margin-left: 10px;
  }

  .content-image {
    width: 100%; /* Makes the image fit within the content box */
    max-width: 100%; /* Ensures it doesn't exceed the container */
    height: auto; /* Maintains aspect ratio */
    display: block; /* Prevents unwanted gaps */
    border-radius: 8px;
    object-fit: contain; /* Ensures it scales properly */
  }

  .eval-image {
    width: 70%; /* Makes the image fit within the content box */
    max-width: 100%; /* Ensures it doesn't exceed the container */
    height: auto; /* Maintains aspect ratio */
    display: block; /* Prevents unwanted gaps */
    border-radius: 8px;
    object-fit: contain; /* Ensures it scales properly */
  }
</style>
