# Harvard CaseLaw sample dataset #

## Overview ##
This dataset consists of 500 legal cases from the [New Mexico court of appeals](https://case.law/download/bulk_exports/latest/by_jurisdiction/case_text_open/nm/). 
Each case appeals some previous court ruling, e.g., a district court or another court of appeals. 
Therefore, the possible outcomes of each case:
the previous ruling is kept as is (AFFIRM);
the previous ruling is changed/annulled (REVERSE);
some parts of the previous ruling are kept and some are changed (MIXED);
the appeal is dismissed (we consider it to be a type of AFFIRM).

The goal of this dataset is to train models that can automatically identify to which of the 4 above groups a case belongs. Once trained, such model will enable us to label large volumes of data (the full CaseLaw dataset being > 40 mio cases).

The original cases are in plain text format. Their length vary (min 2413 symbols, max 142223 symbols, average 25872 symbols), and their complexity vary. 
All cases follow more or less the same structure: 
first comes a short summary, 
then come the facts of what happened before, both in terms of the original events that lead to the first trial, and in terms of what were previous court rulings on the case;
then comes the so-called legal reasoning part, in which the judges justify their decision; this is the most interesting part from the NLP point of view, and this is what each case is all about; this part can get very complex and lengthy, and it may as well restate some of the facts;
finally, there is the conclusion section that contains the final ruling of the court;
in some cases conclusion is followed by an additional section, sort of an Appendix, that may contain comments, dissenting opinions (see below) etc. For now I am ignoring it.

Each section may or may not have a header (Facts/Background/Discussion/Conclusion). The one consistent marker that can be used is that conclusion, and therefore the main part of the case, ends with a phrase IT IS SO ORDERED. 
Most importantly, the sections do not clearly distinguish between facts and legal reasoning. The judges switch back and forth between restating the facts and performing legal reasoning, and some sentences may contain both facts and reasoning. How to fully annotate a legal case like that on the sentence level is a problem of its own.

## The Dataset ##
Annotated cases are stored in json format, one file per case. Each file contains the full text of the case, and 2 types of annotations:
__1. document-level annotation__
Each case is assigned one of the 4 above categories; the distribution of classes is
AFFIRM: 240, REVERSE: 159, MIXED: 101.

Those cases that affirm in part and reverse in part, as well as cases that address only some of the points of the appeal while ignoring the rest are assigned the label MIXED.
Some cases are repeated appeals, i.e., they are appealing the decision of the previous appeal. Sometimes they will explicitly state both previous decisions, e.g., *we affirm the district court decision and reverse the court of appeals*. In such cases taken as label is the decision with regards to the most recent ruling, e.g., REVERSE.
Some cases come to an unambiguous affirm/reverse decision, but after the outcome is announced, one or more judges explain in which way they personally disagree with the ruling (*they dissent*). For example, the court affirms on all accounts, but one judge says he would partially reverse. SUch dissenting opinions are ingnored, and only the overall decision is annotated (in this example AFFIRM).


__2. sentence-level annotation__
Sentences that actually contain the decisions to affirm/reverse/dismiss are annotated with the OUTCOME label (*Accordingly, we affirm.*). Such sentences usually appear at the beginning of the case (in a sort of a summary) and in the Conclusion section; sometimes the overall decision or parts of the decision also appear in the body of the case; I annotated all such sentences. 
