# definition-term-pairs-dataset
Datasets of definition-terms pairs
wiki_definitions_improved.xml.gz

* To quickly view the contents use the `zless` command e.g. `zless wikipedia/wiki_definitions_improved.xml.gz` 
* This repo includes data from the wikipedia, planetmath and the stacks project websites.
* On way to open the files using the `lxml` Python library is the following:
    ```python
   def get_wiki_pm_stacks_data(cfg):
    wiki = []
    with gzip.open(os.path.join(cfg['base_dir'], cfg['wiki_src']), 'r') as xml_fobj:
        def_xml = etree.parse(xml_fobj)
        for art in def_xml.findall('definition'):
            data = (art.find('.//dfndum').text, '', art.find('.//stmnt').text)
            wiki.append(data)

    plmath = []
    with gzip.open(os.path.join(cfg['base_dir'], cfg['pm_src']), 'r') as xml_fobj:
        def_xml = etree.parse(xml_fobj)
        for art in def_xml.findall('article'):
            plmath.append(split_fields(art))

    stacks = []
    with gzip.open(os.path.join(cfg['base_dir'], cfg['stacks_src']), 'r') as xml_fobj:
        def_xml = etree.parse(xml_fobj)
        for art in def_xml.findall('article'):
            try:
                stacks.append(split_fields(art))
            except AttributeError:
                print('The name of the problematic article is: {}'.format(art.attrib['name']))

    text_lst = wiki + plmath + stacks
    random.shuffle(text_lst)
    return text_lst 
    ```
