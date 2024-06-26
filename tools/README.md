# Tools for working with bids-website

This README file describes the tools under the `/tools` directory of the bids-website repository.

## Adding Steering Group meeting notes

1. Install all Python requirements under `tools/requirements.txt`
1. Install pandoc
1. Create a folder called `inputs` under `tools`
1. Nest a folder with the year corresponding to the meeting notes you want to add (e.g., `2023`) in `inputs`
1. Getting the meeting notes
    1. Go to the Google Drive folder where the meeting notes you want to add are located
    1. See if you can make minor formatting tweaks (removing weird formatting, cleaning up tables, etc.)
    1. Download them as `.docx` files
    1. Place all files in your "year" folder that you created above
1. From the command line, navigate to `tools`, and then run `make all`
1. Clean up, check the newly added files, commit

## Update the Gantt chart of completed BEPs

There are two ways of creating Gantt charts for a BIDS timeline.
First install all Python requirements under `tools/requirements.txt`.
Then, use one of the following two methods:

1. `python -u tools/bep_gantt_html_create` creates a file: `_pages/bids_timeline.html` that you can inspect in your browser.
   This file depends on the `tools/timeline.csv` data, which you should keep up to date.
1. `python -u tools/bep_gantt_mermaid_insert` directly inserts code for a "mermaid" Gantt chart into the Get Involved page of the website.
   You can inspect how that chart looks by building the website using Jekyll and serving locally.
   This code depends on data from the files `beps_completed.yml` files in the `_data` directory.
   You should keep that file up to date.

## Updating grant writing kit info

### Citation Count

This script queries the [OpenCitations](https://opencitations.net/index/coci) API
to get the number of papers that cited a list of papers related to the [BIDS](https://bids.neuroimaging.io/) data organization standard.

The resulting citation count is then visualized using [Plotly](https://plotly.com/python/).

### Requirements

To install the required packages, run the following command in your terminal:

```bash
pip install -r requirements.txt
```

### Usage

1. Clone the repository to your local machine.

2. Open the `count_citation.py` file and replace `YOUR-OPENCITATIONS-ACCESS-TOKEN` with your OpenCitations access token.

3. Run the script:

```bash
python count_citation.py
```

4. The citation count for each paper will be saved in a TSV file named `count_citation.tsv`
   in the same directory as the script.

5. A bar chart of the citation count will be displayed using your default browser.
