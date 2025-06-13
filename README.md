# RAG-custom-library
This system adds flexibility to a conventional library catalog search system, where the use can express his/her request in plain English.

This example uses a local QwQ language model (qwq-32b-q8_0.gguf). QwQ is the reasoning model of the Qwen series. This example runs the inference on 'llama_cpp'. Llama.cpp is a software library written in C/C++ designed for efficient inference of large language models.

## EXCERPT:
1.user enters his/her question in plain English, for the type of book the user is wanting to borrow from the library:
```
<Question>
Tell me books that are poems in Bulgarian
</Question>
```

2.the system responds in plain English the name of book(s) that match user's request:
```
<ResponseToUser>
A notable collection of Bulgarian poems is
"<xref href=\"https://www.gutenberg.org/ebooks/2790\">христо ботев, стихотворения</xref>" by Khristo Botev.
This work contains poems written during Bulgaria's struggle for independence from Ottoman rule in the late 19th century.
It explores themes of patriotism, sacrifice, and national identity,
including renowned pieces like "On Parting" and "The Hanging of Vassil Levski."
The text is available in Bulgarian via Project Gutenberg.

No other Bulgarian poetry collections were found in the provided factual statements.
</ResponseToUser>
```

## LLM prompt:
The following is example of the prompt we used to implement this RAG (retrieval augmented generation) system.

We used a TfIdf relevance scoring via Lucene (https://lucene.apache.org/) which delivered higher Precision&Recall than VectorDBs.
```
<|im_start|>user
You are a user agent. If you do not know the answer, reply I don't know, don't make things up.
You need to follow these instructions strictly:
1.Your job is to generate the response to the user, utilizing the following items:
 1.1.User's question, marked up by the tags <Question> and </Question> ;
 1.2.Factual statements marked up by the tags <FactualStatements> and </FactualStatements> .
2.Mark up your answer by tags <ResponseToUser> and </ResponseToUser>.

<Question>
Tell me books that are poems in Bulgarian
</Question>

<FactualStatements>
https://gutenberg.org/ebooks/2790                      x   go!         donate       about ▼   about project gutenberg  contact us  history & philosophy  kindle & ereaders  help pages  offline catalogs  donate     frequently downloaded  main categories  reading lists  search options     frequently downloaded  main categories         project gutenberg     76,104 free ebooks     2 by khristo botev      христо ботев, стихотворения by khristo botev                                                         "христо ботев, стихотворения" by khristo botev is a collection of poems written during the late 19th century, specifically around the time of bulgaria's struggle for independence from ottoman rule. this work encapsulates themes of patriotism, sacrifice, and the emotional turmoil of a nation yearning for freedom. botev, a notable figure in bulgarian literature and a revolutionary, uses his poetry to convey deep feelings of love for his homeland, the pain of separation,    ... read more    and the noble quest for liberty. the collection consists of various poignant pieces showcasing botev's mastery of language and emotional depth. through intimate verses like "on parting," the poet expresses a heartfelt farewell to his mother as he embarks on a fight for freedom, while in "elegy," he reflects on the pain and suffering of his people under oppression. the poem "the hanging of vassil levski" pays tribute to a national hero, emphasizing themes of martyrdom and national identity. overall, botev's work captures the essence of the bulgarian fight for independence, intertwining personal grief with national struggle, and leaving a lasting impact on bulgarian literature and cultural consciousness. (this is an automatically generated summary.)   show less     read or download for free              how to read    url  size              read now!  https://www.gutenberg.org/ebooks/2790.html.images  74 kb          epub3 (e-readers incl. send-to-kindle)  https://www.gutenberg.org/ebooks/2790.epub3.images  93 kb             epub (older e-readers)  https://www.gutenberg.org/ebooks/2790.epub.images  93 kb             kindle  https://www.gutenberg.org/ebooks/2790.kf8.images  197 kb             older kindles  https://www.gutenberg.org/ebooks/2790.kindle.images  197 kb             plain text utf-8  https://www.gutenberg.org/ebooks/2790.txt.utf-8  66 kb          download html (zip)  https://www.gutenberg.org/cache/epub/2790/pg2790-h.zip  91 kb           there may be more files related to this item.        similar books         readers also downloaded…             in browsing: literature             in browsing: poetry          about this ebook          author   botev, khristo, 1848-1876    title  христо ботев, стихотворения   note  the poems of hristo botev   contents  on parting (in 1868) -- to my first love -- sharing -- elegy -- to my mother -- to my brother -- in the tavern -- hadji dimitar -- my prayer -- a cloud of darkness has appeared -- stranger -- eloped -- the struggle -- st. george's day -- a patriot -- to her -- haidouks (father and son) -- why am i not...? -- a message -- the hanging of vassil levski.   credits  produced by snezhina gileva    language  bulgarian    loc class   pg: language and literatures: slavic (including russian), languages and literature     subject   poetry     category  text    ebook-no.  2790    release date  aug 1, 2001    most recently updated  apr 19, 2013    copyright status  public domain in the usa.    downloads  438 downloads in the last 30 days.       project gutenberg ebooks are always free!                      about project gutenberg    privacy policy    permissions    terms of use    contact us   help
..
..
</FactualStatements>
|im_end|>
<|im_start|>assistant
```



