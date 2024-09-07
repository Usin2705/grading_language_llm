## This is the public record for the paper: Automated content assessment and feedback for Finnish L2 learners in a picture description speaking task. Submitted to INTERSPEECH 2024 conference.

A permanent record is available in [Zenodo](https://doi.org/10.5281/zenodo.11385109)

This record is published for the reproducibility of the experiments described in the paper.

The data relating to DigiTala is available in [Kielipankki](https://www.kielipankki.fi/corpora/digitala/), with a [persistent identifier](http://urn.fi/urn:nbn:fi:lb-2024013001). Unfortunately, we cannot share personal data, so you will need to request data directly from Kielipankki. Our file contains the ID which will match the ID in the data. The task for this experiment is task 18.

## Data

All data is in the summary Excel file, but we also include a CSV file for the grading data.

The results from the **harsh** and **lenient** models are located in the 'harsh output' and 'lenient output' sheets. These contain the following columns: 'student', 'transcript_asr', 'grade', 'explanation', 'feedback', 'full_answer', 'completion_object', and 'instruction'.

The results from the **standard**, **transcript**, **0-shot grade 1**, and **GPT-4V** models are located in the 'standard'_output', 'transcript_output', 'zeroshot_output', 'gpt4v_output' sheets. These contain the following columns: 'student', 'transcript_asr', 'grade', 'full_answer', 'completion_object', and 'instruction'. The transcript model used human transcript, which is only available via Kielipankki.

The re-graded grade in our data should replace the original grade in DigiTala, as our experts have more expertise in language assessment.


The names in the ASR transcript are redacted as [name].

## LLM prompt

Details of all prompts are available in the Excel file and Jupyter Notebook files. Most of the prompts follow this structure, with items within <> bracket is customised instruction or examples:

> Task: Based on the provided transcript (made by an automatic speech recognition model) of the user's description of a picture, assign a grade as 'Good', 'Average', 'Bad' or 'None' for the task completion/task achievement. This is the task question given to the user:
> 
> \<The task description\>
> 
> Mistakes in pronunciation, fluency, vocabulary and grammar errors are ignored and do not affect the grade. <criteria for 'Good' grade>. The text is a transcript made by ASR model, so there should be no punctuation.
> 
> Explain your grading in detail and then give the grade. \<instruction for correct feedback, if any\>. Provide your improved version in Finnish inside the '@' tag like this: @Improved version in this tag@
> 
> Grading examples:  
> User:\<Good answer\>  
> System: [Explanation.]Good. @Improved version@  
> User:\<Average answer\>  
> System: [Explanation.]Average. @<improved version>@  
> User:\<Good answer\> 
> System: [Explanation.]Good. @Improved version@  
> User:\<Average answer\> 
> System: [Explanation.]Average. @<improved version>@  
> User:\<Bad answer\>  
> System:[Explanation.]Bad. @<improved version>@  
> 
> Grading Criteria:  
> \<Grading criteria for Good\>  
> \<Grading criteria for Average\>  
> \<Grading criteria for Bad\>  
> 
> This is the description of the picture: \<picture description in text\>  


Code implementation for LLM grading

To reproduce the same result, we recommend using the same Python script. Remember to set the temperature to 0 and the seed number to 1011. Theoretically, you could reproduce the result identical to our experiment if you get the same hardware infrastructure in the OpenAI server. Unfortunately, at the time of the experiment, there is no option in OpenAI API to select the hardware.

The code request for ChatGPT should look like this: 

```Python
completion = client.chat.completions.create(
  model="gpt-4-0613",
  temperature = 0, # Aim for deterministic output, no randomness
  seed = 1011, # Aim for deterministic output, no randomness
  messages=[
    {"role": "system", "content": f"{instruction}"},
    {"role": "user", "content": transcript_asr}
  ]
)
```
 

with `instruction` is a string variable that can be found in **myutils.py** (`TASK_18_GRADING_XXXXX`)

The **gpt_grading.ipynb** file contains the full code for running the grading. The secret file is just the OpenAI API.

## Code implementation for evaluation

We use this line of code to calculate Quadratic Weighted Cohen's Kappa:

`kappa = metrics.cohen_kappa_score(y_true, y_pred, weights='quadratic')`

The **raters_agreement.ipynb** file contains the full code for running the evaluation.

For more information, don't hesitate to contact nhan.phan@aalto.fi

## Citation

Phan, N., von Zansen, A., Kautonen, M., Voskoboinik, E., Grosz, T., Hilden, R., Kurimo, M. (2024) Automated content assessment and feedback for Finnish L2 learners in a picture description speaking task. Proc. Interspeech 2024, 317-321, doi: 10.21437/Interspeech.2024-1166

```tex
@inproceedings{phan24_interspeech,
  title     = {Automated content assessment and feedback for Finnish L2 learners in a picture description speaking task},
  author    = {Nhan Phan and Anna {von Zansen} and Maria Kautonen and Ekaterina Voskoboinik and Tamas Grosz and Raili Hilden and Mikko Kurimo},
  year      = {2024},
  booktitle = {Interspeech 2024},
  pages     = {317--321},
  doi       = {10.21437/Interspeech.2024-1166},
}
