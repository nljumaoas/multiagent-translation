<script>
  const ten_008 = [
    { src: "ten008_raw.jpg", caption: "Original image" },
    { src: "ten008_maskB.png", caption: "Text segmentation mask" },
    { src: "ten008_clusters.png", caption: "Filtered clusters (color-coded)" },
    { src: "ten008_boxes.png", caption: "Ordered text boxes" }
  ];
  const processing_eval = [
    { src: "Levenshtein.png", caption: "Levenshtein distance, visualized" },
    { src: "split_boxes.png", caption: "Green/blue boxes correspond to baseline/observed boxes, respectively" }
  ];
  const processing_results = [
    { src: "TEA_dist.png", caption: "Distribution of accuracy metric results by page" },
    { src: "PT_dist.png", caption: "Distribution of processing times by page" }
  ];
</script>

<main>
  <h1>Developing a LLM-Driven Multi-Agent Framework for Multimodal Translation</h1>

  <p class="authors">
    <a href="https://github.com/fjiang316">Feiyang Jiang *</a>, <a href="https://github.com/KristinaWu23">Kristina Wu *</a>, 
    <a href="https://github.com/nljumaoas">Nick Jumaoas *</a>, <a href="https://cseweb.ucsd.edu/~haozhang/">Hao Zhang</a>
    <br> University of California, San Diego
  </p>

  <p class="subtitle">2025 Data Science Senior Capstone</p>

  <div class="buttons">
    <a href="https://drive.google.com/file/d/1kaKTGjza_5TAwowdz6FN26d_Bm8c3j_E/view?usp=drive_link" class="btn"> Paper</a>
    <a href="https://github.com/nljumaoas/dsc180b" class="btn"> Code</a>
    <a href="https://drive.google.com/file/d/1SGo3LI8U6_L-Rc5T-r2pMld7FOGKrkRS/view?usp=drive_link" class="btn"> Poster</a>
    <a href="https://github.com/mantra-inc/open-mantra-dataset" class="btn"> Data</a>
    <a href="https://drive.google.com/drive/folders/1nAKffHdOa7ws3KfeFXhdoTq7wIqyholz?usp=sharing" class="btn"> Demo</a>
  </div>

  <section class="content">
    <h2>Motivation</h2>
    <p>The increasing global demand for translated content, particularly in multimedia formats like manga, comic book, and literary works, has highlighted significant challenges in maintaining both efficiency and quality in translation processes. Traditional manual translation workflows, which rely heavily on human translators, editors, and typesetters, often struggle with scalability and consistency when handling large volumes of content that combines both textual and visual elements. This challenge becomes particularly acute in the context of manga translation, where cultural nuances, visual context, and textual accuracy must be carefully balanced by skilled professionals, leading to substantial time investments and increased production costs.</p>
    <p>Our goal is to combine recent advancements in multi-agent translation systems and context-aware multimodal translation, leveraging the increasing power of LLMs and VLMs to create a manga translation pipeline that has both convenience of machine translation and the quality of human translation. In doing so, we aim to demonstrate the potential of modular multi-agent frameworks to flexibly accommodate a wide variety of media, which we believe can facilitate the globalization of translated content at affordable costs, allowing for the enrichment of people all around the world.</p>
    
    <h2>Dataset</h2>
    <p>Our primary baseline dataset comes from OpenMantra (<a href="#openmantra-reference">Hinami et al.</a>), an automatic manga translation project that provides 214 manually translated and annotated manga pages for use as a benchmark for future research. </p>
    <div class="ten008-item">
        <img class="eval-image" src={"dataset.png"} alt="Image" />
        <div class="caption">{"OpenMantra text field annotations (excerpt), visualized"}</div>
    </div>
    <p>Notably, although we aim to match the general format for evaluation purposes, our identified text fields have a much tighter fit to facilitate typesetting. Additionally, while the English translation of the pictured text is used to evaluate the overall accuracy of our translation framework, we expect differences due to subjective measures of fluency and reader perception.</p>

    <h2>Methods</h2>
    <p>Our innovated method pipeline involves three stages with the collaboration of agents, beginning with page processing where text is recognized and clustered, then moving on to leverage multi-agent collaboration for translation, and at last the typesetting stage to return a translated version of the page with translations replacing the original texts.</p>
    <img src="method_pipeline.png" alt="Method Pipeline Diagram" class="content-image">

    <section class="processing subsection">
      <h3>Manga-Specialized Preprocessing</h3>
      <div class="ten008-container">
        {#each ten_008 as image}
          <div class="ten008-item">
            <img class="ten008-img" src={image.src} alt="Image" />
            <div class="caption">{image.caption}</div>
          </div>
        {/each}
      </div>
      <p> In the initial preprocessing stage, we implement a pipeline designed to identify the positions of text boxes and extract text from them.</p>
      <ul>
        <li><strong>Text Segmentation: </strong> Creates a mask using a text segmentation model specialized for manga, robust against cases such as oddly shaped text fields and non-text elements within speech bubbles.</li>
        <li><strong>Clustering: </strong> Groups potential text into clusters using the OPTICS algorithm.</li>
        <li><strong>Speech Bubble Filter: </strong> Filters out text outside speech bubbles to match baseline data by analyzing the background color of the text.</li>
        <li><strong>Page Element Ordering: </strong> Sorts clustered text boxes into reading order using the Magi model.</li>
        <li><strong>Text Extraction: </strong> Extracts text using MangaOCR, designed to be robust against scenarios specific to manga, such as text overlaid over images or accompanied by furigana, wide varieties in font style and size, and different reading orientations.</li>
        </ul>
    </section>

    <section class="translation subsection">
      <h3>Multi-agent Context-aware Translation Design</h3>
      <p> In this stage, we introduce a multi-agent system for context-aware translation, focusing on visual-heavy content like manga and comic books. The system combines Visual Language Models (VLMs) and a multi-agent collaborative framework to enhance translation accuracy and coherence, ensuring linguistic precision and contextual relevance.</p>
      <ul>
        <li><strong>Visual Context Analysis: </strong> A Visual Language Model (VLM) analyzes the page's visual context, extracting key elements like characters and settings. This information helps inform the translation process, ensuring the translation is aligned with the visual nuances crucial to the narrative. For our demonstration pipeline, we uses llava 7b as the VLM agent. However, it is a small sized model with limited ability for interpretation and reasoning. If replaced with other stronger VLMs, the performance would be better.</li>
        <li><strong>Multi-agent Translation Framework: </strong> The translation is handled by a multi-agent system with three specialized agents: the Linguistic Specialist Agent for grammatical accuracy, the Cultural Context Specialist Agent for cultural relevance, and the Visual Context Specialist Agent for aligning the translation with visual elements. Each agent contributes to a well-rounded translation. </li>
        <li><strong>Incorporation of Historical Context: </strong> For series with continuous storylines, the system incorporates historical context by tracking narrative elements across multiple pages. This ensures consistency in character names, plot, and themes, maintaining coherence throughout the series. </li>
        <li><strong>Agent Collaboration and Verification: </strong> The three agents engage in a round-robin process, reviewing and critiquing each other's translations. This collaborative approach helps resolve discrepancies and refine the final translation, ensuring it is linguistically accurate, culturally appropriate, and visually coherent. This ensures the quality of final translation. </li>
        <li><strong>Fast Translation for Demo: </strong> Although the regular translation process involves agent double checking on final translation for more natural outputs, this slows down latency and adds to the cost, which would not be convinient for users who want instant access and don't mind the translation quality that much. In order to accommodate the need of this group, we enabled fast translation mode where early stop is implemented based on complexity of text and context. </li>
        </ul>
    </section>

    <section class="typesetting subsection">
      <h3>Typesetting</h3>
      <p>In the final stage of our pipeline, we integrate translated text back into manga pages while preserving visual aesthetics and readability. Our system automates text removal and adaptive reformatting to ensure a natural and high-quality presentation.</p>
      <ul>
        <li><strong>Text Detection and Removal:</strong> Based on our preprocessing identification, we implement the simplest and most effective masking approach using rectangular masks on precisely identified text regions. This targeted approach creates clean white spaces for translations while preserving bubble structures and surrounding artwork.</li>
        <li><strong>Adaptive Text Placement:</strong> The system symmetrically expands text regions horizontally by 16 pixels while maintaining the center point. Text is dynamically wrapped, sized (22px base, 14px minimum) and centered with white outlines for optimal readability across various background elements.</li>
      </ul>
    </section>

    <h2>Evaluation</h2>
    <p>The evaluation of the pipeline is separated into two major parts, one on the accuracy of page processing stage in capturing the correct position of texts and extracting the text accurately, the other part is in assessing the translation quality in context awareness and nuance. To effectively assess the accuracy, coherence, and context-awareness of the multi-agent translation system, we took a combination of automatic metrics such as BERTScore, as well as human evaluation to determine how well the system preserves the emotional nuance and narrative consistency.</p>
    
    <section class="Processing Evaluation">
        <h3>Processing Stage Evaluation</h3>
        <div class="ten008-container">
            {#each processing_eval as image}
                <div class="ten008-item">
                <img class="ten008-img" src={image.src} alt="Image" />
                <div class="caption">{image.caption}</div>
                </div>
            {/each}
        </div>
        <p> The preprocessing stage is evaluated on two primary elements:</p>
        <ul>
            <li><strong>Text Extraction: </strong> 
                <ul>
                    <li>Uses Levenshtein distance ratio to penalize common errors (misidentified page elements) as deletions while accurately evaluating the accuracy of the rest of the extracted text.</li>
                    <img src="levenshtein_ratio.png" alt="Formula used for Levenshtein Ratio" class="ten008-img">
                    <li>Also used by comparable projects (<a href="#lippmann-reference">Lippmann et al.</a>), allowing for comparison in performance.</li>
                </ul>
            </li>
            <li><strong>Text Box Position: </strong> 
                <ul>
                    <li>Implemented box-center check to assess accuracy despite systematic discrepancies with baseline text boxes.</li>
                    <li>Major discrepancies are tighter box fit and split text boxes in our model (see figure above), which facilitate typesetting later in the pipeline.</li>
                </ul>
            </li>
        </ul>  
    </section>

    <section class="Translation">
      <section class="bertscore">
        <h3>BERTScore and Human Evaluation</h3>
        <p>To effectively assess the accuracy, coherence, and context-awareness of the multi-agent translation system, we took a combination of automatic metrics such as BERTScore, as well as human evaluation to determine how well the system preserves the emotional nuance and narrative consistency.</p>
        <img src="bert_score_formula.png" alt="Formula used for BertScore" class="eval-image">
        <ul>
          <li><strong>BERTScore</strong>: Capture semantic similarity, focusing on contextual accuracy and nuance. Evaluated by Bert F1-score. </li>
          <li><strong>Human Evaluation</strong>: Monolingual and bilingual experts rating translation for fluency, accuracy, proximity to released human translation and cultural appropriateness, choosing one best overall translation from the results of our model and one randomly selected baseline model.  </li>
        </ul>
        <p>Below is an example of our human evaluation survey question. We have a total of 4 different versions of the survey, each consists of ten randomly selected pages with one of the option being our model's output and the other being a randomly selected baseline's output to reduce bias. The final percentage of preference was taken as metrics.</p>
        <img src="survey_example.png" alt="example of human evaluation survey" class="eval-image">
      </section>

      <section class="machine eval">
        <h3>Machine Evaluation</h3>
        <p>To systematically assess the quality of translations, we incorporate Machine Evaluation using the ChatGPT web application with the GPT-4o model as an automated evaluator. This approach allows us to efficiently compare different translation outputs at scale.</p>
      </section>
    </section>

    <section class="challenge of evaluation on typesetting">
      <h3>Challenge of Evaluation on Typesetting Stage</h3>
      <p>Unlike translation accuracy, which can be measured with structured metrics, typesetting evaluation lacks automated methods. Assessing readability, artistic consistency, and text alignment remains largely subjective.</p>
      <p>Existing tools can assist with text placement, but no standardized framework exists for systematically evaluating typesetting quality. Factors like font choice, text integration with artwork, and complex layouts pose significant challenges for automation.</p>
      <p>To address this gap, our work explores approaches for structured typesetting evaluation, laying the groundwork for future research.</p>
    </section>

    <h2>Results</h2>

    <section class="Processing Results">
        <h3>Processing Stage Results</h3>
        <div class="ten008-container">
            {#each processing_results as image}
                <div class="ten008-item">
                    <img class="ten008-img" src={image.src} alt="Image" />
                    <div class="caption">{image.caption}</div>
                </div>
            {/each}
        </div>
        <p> After processing the entire OpenMantra dataset and comparing our output to the baseline, we observed the following results:</p>
        <ul>
            <li><strong>Text Extraction: </strong> 
                <ul>
                    <li>Achieves accuracy of 86.3%, with errors mostly coming from misidentified text fields that escape the filter.</li>
                    <li>Outperforms <a href="#lippmann-reference">Lippmann et al.</a> by almost 10%, primarily due to the implementation of the speech bubble filter.</li>
                    <li>Text extracted from actual dialogue fields is almost always perfectly extracted by the OCR system.</li>
                </ul>
            </li>
            <li><strong>Text Box Position: </strong> 
                <ul>
                    <li>22% of pages contain at least one text box not included in the baseline data.</li>
                    <li>Almost all of these are misidentified sound effects, which do not appear to significantly affect translation quality.</li>
                </ul>
            </li>
            <li><strong>Latency: </strong> 
                <ul>
                    <li>Model requires an average of about six seconds of processing time per stage, primarily allotted to clustering.</li>
                    <li>Pages requiring extraordinarily long processing times usually have large amounts of sound effects, which add significant computational load to the clustering algorithm.</li>
                </ul>
            </li>
            <li>Please note that these results come from processing using an A40 GPU; with increased computational power, the latency is expected to be significantly lower.</li>
        </ul>
        <div class="ten008-item">
            <img class="ten008-img" src={"latency_table.png"} alt="Image" />
            <div class="caption">{"Latency breakdown by stage"}</div>
        </div>
    </section>

    <section class="Translate result">
      <section class="bertscore">
        <h3>Performance and BERTScore</h3>
        <img src="bert_result.png" alt="table of metrics evaluation" class="eval-image">
        <p>We evaluate our pipeline against three baselines that are effective in I2I translation tasks:</p>
        <ul>
          <li><strong>Comparison to mono-agent baseline</strong>: our multi-agent structure outperform the baseline with both models (llama3.1:8b and gpt-4-turbo) </li>
          <li><strong>Comparison to Google Translate</strong>: out multi-agent structure using gpt-4-turbo outperforms google translate baseline. </li>
        </ul>
        <p>This finding has been validated with human evaluation with results shown below.</p>
      </section>

      <section class="human eval">
        <h3>Human Evaluation Result</h3>
        <img src="human-eval-result.png" alt="human evaluation result" class="eval-image">
        <p>As shown in the plot, our model achieved a 67% winning rate against mono-agent structure, and a 86% winning rate against google translate.</p>
      </section>

      <section class="machine eval result">
        <h3>Machine Evaluation</h3>
        <p>The results from the GPT-4o-based Machine Evaluation indicate that our model performed favorably in comparison to baseline models:</p>
    
        <ul>
          <li><strong>90% preference</strong> over Google Translate, suggesting improvements in fluency and accuracy.</li>
          <li><strong>65% preference</strong> over the mono-agent baseline, reflecting notable enhancements in translation quality.</li>
        </ul>

        <img src="GPT_evaluation.jpeg" alt="LLM evaluation result" class="eval-image">
        <p>These findings suggest that GPT-4o's automated evaluation aligns well with human judgment and provides valuable insights into translation quality assessment.</p>
      </section>
    </section>

    <h2>Conclusion</h2>
    <p>Our multi-agent framework enhances manga translation by improving translation quality and aligning with human reading expectations. By distributing tasks—text detection, contextual translation, and typesetting—our system ensures more natural and readable translations.</p>
    <p>We also developed a specialized typesetting system to seamlessly integrate translated text into manga, addressing formatting challenges often overlooked in automated approaches.</p>
    <p>Challenges remain, including speech bubble resizing and processing latency. Future work will focus on real-time translation and expanding support for other media types.</p>
    <p>This research sets the foundation for sophisticated multimodal translation systems that balance linguistic accuracy with visual presentation.</p>
    
    <h2>References</h2>
    <section id="lippmann-reference">
        <p>Lippmann, Philip, Konrad Skublicki, Joshua Tanner, Shonosuke Ishiwatari, and Jie
            Yang. 2024. “Context-Informed Machine Translation of Manga using Multimodal Large
            Language Models.” <a href="https://arxiv.org/abs/2411.02589" target="_blank">[Link]</a></p>
    </section>
    <section id="openmantra-reference">
        <p>Hinami, Ryota, Shonosuke Ishiwatari, Kazuhiko Yasuda, and Yusuke Matsui. 2021.
            “Towards Fully Automated Manga Translation.” <a href="https://github.com/mantra-inc/open-mantra-dataset" target="_blank">[Link]</a></p>
    </section>
    
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

  .content h3 {
    color:rgb(164, 193, 252);
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
    margin: auto;
  }

  .ten008-container {
    display: flex;
    flex-wrap: wrap;
    gap: 5px; /* Reduce space between items */
    justify-content: center; /* Keep images centered */
    align-items: flex-start; /* Align images on the same horizontal axis */
  }

    .ten008-item {
        text-align: center;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: flex-start;
    }

    .ten008-img {
        width: 100%; /* Ensures images scale to container */
        max-width: 400px; /* Increase max width */
        height: auto; /* Set a fixed height to align them */
        object-fit: cover; /* Maintain aspect ratio */
        display: block;
    }

    .caption {
        margin-top: 2px; /* Removes any default margins */
        font-size: 14px;
        color: #555;
        text-align: center;
    }

    /* Mobile optimization */
    @media (max-width: 600px) {
        .ten008-container {
            flex-direction: column; /* Stack images vertically on small screens */
            align-items: center;
        }

        .ten008-img {
            max-width: 100%; /* Ensure images don't overflow */
        }
    }
  
</style>
