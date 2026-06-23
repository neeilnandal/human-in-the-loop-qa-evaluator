Data Evaluation App: Human-in-the-Loop Annotation Tool for Q&A Responses

A lightweight Streamlit-based data annotation app for evaluating model-generated Q&A responses. The app allows users to review question-answer pairs, mark whether a response has an issue, assign an issue category, filter annotated data, and visualize annotation progress.
This project demonstrates how a simple human-in-the-loop evaluation workflow can be built quickly using Python, Pandas, and Stream lit.

Repository Summary
Project type: Interactive data annotation app
Use case: Q&A model response evaluation
Framework: Streamlit
Core libraries: Python, Pandas
Primary feature: Editable annotation table using st.data_editor
Output: Filtered annotations and basic issue-category visualization

Problem
Evaluating AI-generated responses is not just about whether the model gives an answer. The real question is whether the answer is useful, correct, safe, and complete.
For Q&A systems, model outputs can fail in different ways:
•	The answer may be factually wrong
•	The answer may miss key information
•	The answer may be irrelevant to the question
•	The answer may sound coherent but still be misleading
•	The answer may contain bias or poor reasoning
Raw model responses are difficult to improve unless these failure patterns are labeled clearly. This app creates a simple workflow for turning model outputs into structured evaluation data.

First-Principles View
The real problem is not “build a dashboard.”
The real problem is:
Model teams need a fast way to inspect outputs, label failure modes, and understand where the model is breaking.
A full MLOps platform would be excessive for a small response-review workflow. A spreadsheet alone is easy to use, but weak for structured filtering and visualization. Streamlit sits in the useful middle: fast to build, interactive, and good enough to support early-stage evaluation.

Solution
This app provides an interactive interface where users can annotate model-generated answers.
Users can:
•	View a sample Q&A dataset
•	Mark whether a response contains an issue
•	Select an issue category
•	Filter responses by issue status and category
•	Track annotation progress
•	Visualize issue distribution using a bar chart
The result is a simple but practical evaluation loop for human review of AI outputs.

Key Features
•	Interactive Q&A response review
•	Editable annotation table
•	Checkbox-based issue marking
•	Selectbox-based issue categorization
•	Category-level filtering
•	Annotation progress metric
•	Issue count metric
•	Bar chart showing issue-category distribution
•	Lightweight Streamlit interface

Tech Stack
Area	Technology
Programming Language	Python
Web App Framework	Streamlit
Data Handling	Pandas
UI Components	st.data_editor, st.selectbox, st.metric, st.bar_chart
Use Case	Human evaluation of Q&A model outputs

App Workflow
Load sample Q&A dataset
        |
Display questions and model-generated answers
        |
User marks whether each answer has an issue
        |
User assigns an issue category
        |
App stores updated annotations in an editable dataframe
        |
User filters reviewed responses
        |
App shows metrics and issue-category visualization

Issue Categories
The app supports the following response-quality categories:
Category	Meaning
Accuracy	The answer contains incorrect or hallucinated information
Relevance	The answer does not properly address the question
Coherence	The answer is unclear, inconsistent, or poorly structured
Bias	The answer may contain biased or unfair framing
Completeness	The answer is partially correct but missing important detail

Example Dataset
The app uses a small sample dataset of model-generated Q&A responses. Some answers are intentionally flawed to demonstrate annotation.
Example failure types include:
•	Hallucinated facts
•	Incorrect scientific explanations
•	Partially correct but incomplete answers
•	Humorous but unrealistic responses
This makes the app useful as a small demonstration of response evaluation rather than a production-grade benchmark.

Code Structure
Suggested repository structure:
data-evaluation-app/
│
├── app.py
├── README.md
├── requirements.txt
└── screenshots/
    ├── data-app.png
    
   
    

Installation
Clone the repository:
git clone https://github.com/your-username/data-evaluation-app.git
cd data-evaluation-app
Create and activate a virtual environment:
python -m venv venv
On macOS or Linux:
source venv/bin/activate
On Windows:
venv\Scripts\activate
Install dependencies:
pip install -r requirements.txt

Requirements
Create a requirements.txt file with:
streamlit
pandas

Run the App
Start the Streamlit app:
streamlit run app.py
Then open the local URL shown in the terminal.
By default, Streamlit usually runs at:
http://localhost:8501

Main App Logic
The app starts by creating a sample dataframe containing questions and model-generated answers.
data = {
    "Questions": [
        "Who invented the internet?",
        "What causes the Northern Lights?",
        "Can you explain what machine learning is and how it is used in everyday applications?",
        "How do penguins fly?"
    ],
    "Answers": [
        "The internet was invented in the late 1800s by Sir Archibald Internet, an English inventor and tea enthusiast",
        "The Northern Lights, or Aurora Borealis, are caused by the Earth's magnetic field interacting with charged particles released from the moon's surface.",
        "Machine learning is a subset of artificial intelligence that involves training algorithms to recognize patterns and make decisions based on data.",
        "Penguins are unique among birds because they can fly underwater..."
    ]
}
The dataframe is then extended with annotation columns:
df["Issue"] = [True, True, True, False]
df["Category"] = ["Accuracy", "Accuracy", "Completeness", ""]
The main annotation workflow is powered by Streamlit’s st.data_editor:
new_df = st.data_editor(
    df,
    column_config={
        "Questions": st.column_config.TextColumn(
            width="medium",
            disabled=True
        ),
        "Answers": st.column_config.TextColumn(
            width="medium",
            disabled=True
        ),
        "Issue": st.column_config.CheckboxColumn(
            "Mark as annotated?",
            default=False
        ),
        "Category": st.column_config.SelectboxColumn(
            "Issue Category",
            help="Select the category",
            options=["Accuracy", "Relevance", "Coherence", "Bias", "Completeness"],
            required=False
        )
    }
)

Metrics
The app calculates two simple metrics:
Metric	Description
Number of responses	Count of responses marked as having an issue
Annotation progress	Percentage of responses marked as issues out of total responses
issue_cnt = len(new_df[new_df["Issue"] == True])
total_cnt = len(new_df)
issue_perc = f"{issue_cnt / total_cnt * 100:.0f}%"

Visualization
The app visualizes the distribution of issue categories using a Streamlit bar chart.
df_plot = new_df[new_df["Category"] != ""].Category.value_counts().reset_index()

st.bar_chart(df_plot, x="Category", y="count")
This gives a quick view of where the model is failing most often.

Security and Code Review Notes
This app is low-risk because it runs locally and uses a hardcoded sample dataset. Still, basic security and quality checks matter.
Area	Review
User input	No free-form external input is executed
Data handling	Sample data is created in memory
File uploads	Not enabled in the current version
External APIs	None used
Secrets	No API keys or credentials included
Code execution risk	No dynamic code execution
Deployment risk	Low for local/demo use
Data privacy	Safe with sample data, but real model outputs should be reviewed for sensitive information before upload

Decision Logic
Option	Pros	Cons	Decision
Spreadsheet only	Simple and familiar	Weak interactivity, limited validation, poor visualisation	Not enough
Full annotation platform	Powerful and scalable	Too heavy for a small demo or prototype	Overkill
Streamlit app	Fast, interactive, Python-native, easy to extend	Needs added persistence for production use	Selected

Scientific Evaluation Angle
This app supports the first step of model evaluation: structured human labelling.
A stronger evaluation workflow could add:
•	Inter-annotator agreement
•	Multiple reviewers per answer
•	Confidence scores
•	Reviewer comments
•	Error taxonomy analysis
•	Category-level failure rates
•	Before-and-after model comparison
•	Statistical comparison across model versions
Example research question:
Which error categories appear most frequently in model-generated Q&A responses?
Example metric:
Accuracy issue rate = responses labeled Accuracy / total annotated responses
This makes the project relevant to model evaluation, QA testing, and applied AI reliability.






