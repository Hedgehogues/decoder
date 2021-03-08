# Decoder

Dcode gets any bytes and try transform it to all standart encode. All variants this function returns as list of strings.

[List](https://docs.python.org/3/library/codecs.html#standard-encodings) standart encoding.

I parsed all strings via [ferret](https://github.com/MontFerret/ferret) library with next script:

    LET doc = DOCUMENT('https://docs.python.org/3/library/codecs.html#standard-encodings', { driver: "cdp" })

    WAIT_ELEMENT(doc, 'table[class="docutils align-default"]', 5000)

    LET encodings = ELEMENTS(doc, 'table[class="docutils align-default"]')[4]
    LET cells = ELEMENTS(encodings, 'tbody > tr > td')

    LET N = length(cells)
    LET encode = (
        FOR i IN 0..N
            FILTER i % 3 == 0
            FILTER cells[i] != null
            RETURN ELEMENTS(cells[i], "p")[0]
    )

    RETURN encode
    
Don't forget docker container with chromium browser.

*`latin_1`, `cp037` encodings, as well as, perhaps, some others are able to successfully decode absolutely any sequence of bytes. Remember this.*
