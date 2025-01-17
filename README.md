# SPARQA: question answering over knowledge bases

Codes for paper: "SPARQA: Skeleton-based Semantic Parsing for Complex Questions over Knowledge Bases" (AAAI-2020) [detail](https://aaai.org/ojs/index.php/AAAI/article/view/6426).
If you meet any questions, please email to him (ywsun at smail.nju.edu.cn). 

**Note that SPARQA is updated to SkeletonKBQA. If you are interested in SkeletonKBQA, please see [here](https://github.com/nju-websoft/SkeletonKBQA).**

## Project Structure:

<table>
    <tr>
        <th>File</th><th>Description</th>
    </tr>
    <tr>
        <td>code</td><td>codes</td>
    </tr>
    <tr>
        <td>skeleton</td><td>skeleton bank</td>
    </tr>
    <tr>
        <td>slides</td><td>slides and poster</td>
    </tr>
</table>
 
## Requirements
* [requirements.txt](https://github.com/nju-websoft/SPARQA/blob/master/code/requirements.txt)

## Configuration
* Root of dataset: default D:/dataset. Note that you can edit it in common/globals_args.py. 

**Note that the following files are in baidu wangpan. The extraction code of all files is kbqa.**

## Common Resources
* [Eight Resources](https://pan.baidu.com/s/1__BBXhEvUuRfqdurofHooQ): GloVe (glove.6B.300d), Stanford CoreNLP server, SUTime Java library, BERT pre-trained Models, and four preprocessing files(stopwords.txt, ordinal_fengli.tsv, unimportantphrase, and unimportantwords). unzip and save in the root.
* Two version Freebase: [latest version](https://pan.baidu.com/s/1CCxljj_yH9S3Y4Zeh6epmw) and [2013 version](https://pan.baidu.com/s/1FWwv1R_7JtO_mpk_6pL_TQ). Next, download a virtuoso server and load the KBs. You can also download the KBs from [freebase site](https://developers.google.com/freebase). The [file](http://ws.nju.edu.cn/blog/2017/03/virtuoso%E5%AE%89%E8%A3%85%E5%92%8C%E5%AF%BC%E5%85%A5%E6%95%B0%E6%8D%AE/) is helpful, if you meet questions.

## Specific CWQ 1.1 Resources
* [CWQ 1.1 dataset](https://pan.baidu.com/s/1N_WBCmoQIvNCk_W4oFHeKA): skeleton parsing models, word-level scorer model, sentence-level scorer model. unzip and save in the root.
* [Lexicons](https://pan.baidu.com/s/146e7C4LCrNiQJp6urZU_ZQ): entity-related lexicons and KB schema-related lexicons. unzip and save in the root.

## Specific GraphQuestions Resources
* [GraphQuestions dataset](https://pan.baidu.com/s/106vC73W9WKXyuuFcaoPIuQ): Skeleton Parsing models, Word-level scorer model. unzip and save in the root.
* [Lexicons](https://pan.baidu.com/s/1VfF7O0TDRCKiZxqxRpQ8fQ): Entity-related Lexicons and KB schema-related lexicons. unzip and save in the root.

## Run SPARQA Pipeline
The pipeline has two steps for answering questions: 

* (1) KB-indenpendent graph-structured ungrounded query generation.
* (2) KB-dependent graph-structure grounded query generation and ranking.

See running/freebase/pipeline_cwq.py if run CWQ 1.1.
See running/freebase/pipeline_grapqh.py if run GraphQuestions.
Below, an example on GraphQuestions.

**Note that the steps are not friendly. To understand easliy, we provided samples of these steps in the output_graphq folder.**

### Specific-dataset Configuration

* Set datset in the common/globals_args.py: q_mode=graphq. (note that q_mode=cwq if CWQ 1.1)
* Set skeleton parsing in the common/globals_args.py: parser_mode=head, which means skeleton parsing. (note that parser_mode=dep, which means dependency parsing).
* Replace the freebase_pyodbc_info and freebase_sparql_html_info in the common/globals_args.py with your local address. (note that 2013 version is for GraphQuestions, and latest version is for CWQ 1.1).

### KB-indenpendent query generation
* Run KB-indenpendent query generation. Setup variable module=1.0. The input: dataset. The output: structure with 1.0 ungrounded graph. We provided sample in output_graphq folder.

### KB-dependent query generation
* Generate variant generation. Set variable module=2.1. The input: structure with 1.0 ungrounded graph. The output: structure with 2.1 grounded graph. We provided sample in output_graphq folder.
* Ground candidate queries. Set module=2.2. The input: structure with 2.1 grounded graph. The output: structure with 2.2 grounded graphs. We provided samples of questions in output_graphq folder. [one sample](https://github.com/nju-websoft/SPARQA/blob/master/slides/274000300.json).
* Rank using word-level scorer. Set module=2.3_word_match. The input: 2.2 grounded graphs.
* Combine sentence-level scorer and word-level scorer. Set module=2.3_add_question_match. The input: 2.2 grounded graphs.
* Run evaluation. Set module=3_evaluation. The input: 2.2 grounded graphs. The output: result. 

## Skeleton Parsing
* SPARQA also provides a tool of parsing. The input is a question. The output is the skeleton of the question. (Now, it only supports English language. Later, it will support Chinese language)
* You can use SPARQA's skeleton parsing to train yourself language. (It need replace the pre-trained models and annotated data with your language)

## Multi-Strategy Scoring
* SPARQA has provided a trained word-level scorer model and sentence-level scorer in dataset folder.

## Oracle Grounded Graph
* We provide the code of offline ways, [oracle graphs of CWQ 1.1](https://pan.baidu.com/s/11138yi_oe3TaV9NiuL6pVQ) and [oracle graphs of GraphQuestions](https://pan.baidu.com/s/1DAcCX2ic-eFYptn3FeEWbg). The way first retrieve oracle graphs (to reduce storage space) and then generate candidate queries from oracle graphs. About oracle graph, please see [this paper](https://www.aclweb.org/anthology/Q16-1010.pdf).
* We can also provide the code of online ways. The way is to generate candidate queries online. The problem is efficiency issue.

## Compare with Baselines
* GraphQuestions: PARA4QA, SCANNER, UDEPLAMBDA.
* CWQ 1.1: PullNet, SPLITQA, and MHQA-GRN. Note that PullNet used annotated topic entities of questions in its KB only setting. SPARQA, an end-to-end method, do not use annotated topic entities. Thus, it is not comparable.

## Citation

	@inproceedings{SunZ0Q20,
	  author    = {Yawei Sun and Lingling Zhang and Gong Cheng and Yuzhong Qu},
	  title     = {{SPARQA:} Skeleton-Based Semantic Parsing for Complex Questions over Knowledge Bases},
	  booktitle = {The Thirty-Fourth {AAAI} Conference on Artificial Intelligence, {AAAI} 2020, The Thirty-Second Innovative Applications of Artificial Intelligence Conference, {IAAI} 2020, The Tenth {AAAI} Symposium on Educational Advances in Artificial Intelligence, {EAAI} 2020, New York, NY, USA, February 7-12, 2020},
	  pages     = {8952--8959},
	  publisher = {{AAAI} Press},
	  year      = {2020},
	  url       = {https://aaai.org/ojs/index.php/AAAI/article/view/6426},
	}

## Contacts
If you have any difficulty or questions in running codes, reproducing experimental results, and skeleton parsing, please email to him (ywsun at smail.nju.edu.cn).

