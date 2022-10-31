# NER
In this repository I will upload my researchs about NER: libraries, use cases, evaluation methods...

## Documents:
#### NER_test.ipynb
It is a notebook where I proposed a new complex method for testing NER performance. On the state of the arte in evaluation NER models, several alternatives are found. From the strictest method: an entity will be well predicted if and only if its string defines a single real entity (the predicted entity and the real entity are exactly the same). To the softest method: each entity will be considered as a set of tokens and those tokens will be strictly checked whether they belong to a real entity or not. 

From here onwards, the "soft" evaluation will be used. Both detection and classification will be evaluated. The evaluation of the classification will be done on the tokens of the well detected entities, i.e. on the TP's of the detection.

In order to carry out this evaluation method, a first strategy was followed: the text is processed while it is being tested. For each text, its entities are extracted. For each predicted entity, the tokens of the text are progressed through until a token belonging to this predicted entity is found. This method therefore allows True Negative tokens to be taken into account, which means that the accuracy can be measured. However, it has problems, since it could be the case that a token in the text coincides with that of the predicted entity, without actually being the token of the entity, i.e. it is a word repeated in the text that is in another position and can therefore cause problems. 
In addition, entities have to be calculated at each execution which causes slow execution.
 - Advantages:
    - True Negatives detection and accuracy measurement.
 - Problems:
    - Repeated words in the text can cause problems.
    - Long time to test new metrics.
 - Solution:
    - Use offset within text to locate entities.
    - Save detected and actual entities on disk.

Given the problems that were encountered, the strategy changed course following the indications of the solution above. Saving the detected and actual entities on disk saved execution time and the use of in-text offset to locate the entities solved the problem of repeated words in the text. In addition, a further improvement was incorporated, which was to eliminate the so-called 'Stopwords' from the evaluation by adding relevance to the detection of the semantic core of the entity.

To locate the entities through the offset, the position of the token within the text has been used as a first approximation. Both Spacy and Flair use their own tokeniser to process the text. Taking the token offset can be problematic as it does not use a common tokeniser. Also, by including Google Cloud NER in the study it is necessary to use an offset at the character level. 

 - Advantages:
    - Speed: detected and real entities pre-computed.
        
  - Problems:
    - True Negative cannot be taken into account in the detection.
    - Using different tokenisers may result in different offsets of the same token.

Not being able to take True Negatives into account in detection is not a major problem as discussed above. However, the tokeniser problem is relevant. That is why the solution was reformulated using the offset on the original text at char level in Spacy, Flair and Google Cloud. 


In the executions, the Flair model was found to be much slower. The SPACY model takes the least time to detect entities. Using this model as a base, Google Cloud takes about 5 times longer than SPACY and FLAIR takes about 20 times longer than SPACY.

