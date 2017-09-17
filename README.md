# Data: Quantitative Perspectives on Fifty Years of the Journal of the History of Biology

Data from the forthcoming paper:

- Peirson, B. R. Erick, Erin Bottino, Julia L. Damerow, and Manfred D. Laubichler. 2017. Quantitative perspectives on fifty years of the 
*Journal of the History of Biology* 50(4).

Unless otherwise specified, data are comma-delimited. Missing values are left empty.

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
Relations between ``article_metadata.csv`` (by DOI) and ``document_authors.csv`` (by author index).

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
