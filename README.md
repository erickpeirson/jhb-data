# Data: Quantitative Perspectives on Fifty Years of the Journal of the History of Biology

Data from the forthcoming paper:

- Peirson, B. R. Erick, Erin Bottino, Julia L. Damerow, and Manfred D. Laubichler. 2017. Quantitative perspectives on fifty years of the
*Journal of the History of Biology* 50(4).

Unless otherwise specified, data are comma-delimited. Missing values are left empty.

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.

## Article metadata

### ``article_metadata.csv``
From JSTOR. DOIs can be used for joins across other tables in this dataset.

- ``Title``: string
- ``Date``: integer
- ``Volume``: integer
- ``Issue``: integer
- ``StartPage``: integer
- ``EndPage``: integer
- ``DOI``: string

### ``author_names.csv``
Maps author indices (used in other tables) to readable names.

- ``Author``: integer (author index)
- ``Name``: string (readable name)

### ``document_authors.csv``
Relations between ``article_metadata.csv`` (by DOI) and ``document_authors.csv``
(by author index).

- ``DOI``: string
- ``Author``: integer

## Geography

> We examined each of the articles in JHB over its entire run, and attempted to
> identify the physical location of the author at the time of publication, and to
> determine locations that were discussed in the article content. To locate
> references to locales, we took a single visual pass over each article and noted
> any references to municipalities, regions, or states, taking care to spend an
> equitable amount of time on each article. We assume that we found a subset of
> the total references to locations. We then found the closest match to that
> location in the GeoNames geographical database (http://www.geonames.org/), and
> recorded the corresponding Uniform Resource Identifier (URI).

> For the sake of consistency, we used current location identifiers and
> geopolitical boundaries, which is in some cases highly anachronistic. For
> example, the name "Czechia" was only officially adopted by the Czech Republic in
> 2016, and the Republic itself has only existed since 1993, yet we have used the
> current term to tag articles published as early as the 1960s that refer to
> (historical) Czechoslovakia. For the present high-level analysis this does not
> have a substantial impact on our results or conclusions. In other studies,
> however, historians incorporating digital geographic data in their research may
> find it fruitful to use regional databases of historical place names (e.g. The
> Historical Gazatteer of England's Place Names [http://placenames.org.uk/]).
> GeoNames itself also has increasing support for historical place names.


### ``article_localizations.csv``
Location tags for article content and and authors.

- ``DOI``: string
- ``Relation``: string (``content`` or ``author``)
- ``GeoNamesID``: integer

### ``location_metadata.csv``
Location information retrieved from [GeoNames](http://www.geonames.org/).

- ``GeoNamesID``: integer
- ``Latitude``: float (degrees north of the equator)
- ``Longitude``: float (degrees east of the prime meridian)
- ``Name``: string
- ``CountryCode``: string (ISO 3166-1 alpha-2)
- ``CountryID``: integer (GeoNames country identifier; missing values are ``-1``)

## Organisms

> We used the LINNAEUS NER model (Gerner et al 2010) to tag references to
> organisms on each page of JHB. NER is a problem in information retrieval in
> which the goal is to identify words or phrases in a text that refer to
> instances of a particular class of entities, such as people, places,
> institutions, or dates. NER is usually achieved through supervised machine
> learning, in which a "training set" of human-annotated documents is used to
> train a classifier. LINNAEUS is a dictionary-based NER application, in which a
> large collection of documents from Medline and PubMed Central that had already
> been tagged with entries from the NCBI Taxonomy database were used to generate
> a lexicon of phrases that refer to specific taxa. LINNAEUS matches both formal
> taxonomic terms (e.g. species binomial names) and common names (e.g. "mouse").

### ``organisms.csv``
Entities recognized on each page.

- ``Entity``: integer (NCBI Taxonomy identifier)
- ``DOI``: string
- ``Page``: integer (0-indexed)
- ``Text``: string (matching word or phrase in document)
- ``Start``: integer (character offset of phrase start)
- ``End``: integer (character offset of phrase end)

### ``taxon_labels.csv``
Human-readable labels for entities in ``organisms.csv``. Taken from the
``ScientificName`` field in NCBI Taxonomy database.

- ``Entity``: integer (NCBI Taxonomy identifier)
- ``Label``: string (usually a binomial name)

## Topics

> Prior to model fitting we applied several preparatory transformations to the
> paginated full text provided by JSTOR. Within each document, we removed running
> headers from each page. We used an MCMC simulation to locate the bibliography
> within each article (if one was present) based on the distribution of specific
> punctuation characters and key terms; if a bibliography was identified, that
> content was excluded from analysis. We tokenized each page at the level of
> individual words, removing all punctuation, whitespace, and numeric characters.
> Since many concepts of interest in JHB are represented by multi-word phrases,
> we extracted two- to six-word phrases by applying (in three sequential passes)
> the criterion $\frac{N_{ij}-5}{N_i+N_j }>0.1N$ where Nij is the number of
> occurrences of the bigram (word i
> followed by word j), Ni is the total number of occurrences of word i, and N is
> the total number of tokens in the whole corpus (Řehůřek and Sojka 2010). After
> conjoining word-parts into phrases as described, we removed tokens of any
> individual words that (a) occur in the Natural Language ToolKit stopwords list
> (Bird et al 2009), or (b) occur on more than 6,000 pages (about 1/4 of the
> corpus).

> We fit a series of topic models to the articles in JHB, treating each page as a
> separate document and varying the number of topics with . We used the
> parallelized collapsed Gibbs sampler implemented in the InPhO Vector Space
> Model package (Murdock 2015), and ran each simulation for 10,000 iterations. In
> each case the simulation converged successfully.

### ``word_labels.csv``
This is the corpus vocabulary; maps "word" indices (integer) to human-readable
labels. Note that many of the "words" in the vocabulary are actually multi-word
phrases.

- ``Word``: integer
- ``Label``: string

### ``topic_page_assignments__k[N].csv``
Posterior probability of topic assignments for each page.

- ``Topic``: integer (0-N)
- ``DOI``: string
- ``Page``: integer
- ``Probability``: float
- ``Assignments``: float (number of words on page assigned to topic)
- ``Characteristic``: float (number of times more likely topic is to occur on
  this page than on a random page in the corpus).

### ``word_topic_assignments__k[N].csv``
Posterior probability of words given each topic.

- ``Topic``: integer (0-N)
- ``Word``: integer (word index)
- ``Probability``: float

### ``topic_article_assignments__k[N].csv``
Data from ``topic_page_assignments__k[N].csv`` aggregated and re-normalized at
the article level.

- ``Topic``: integer (0-N)
- ``DOI``: string
- ``Probability``: float
- ``Assignments``: float (number of words in article assigned to topic)
- ``Characteristic``: float (number of times more likely topic is to occur in
  this article than on a random page in the corpus).
