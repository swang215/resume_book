# resume_book
# frontend
import requests
from xml.parser.expat import ParserCreate
import xml.etree.ElementTree as ET

class DefaultSaxHandler(object):
    def __init__(self, province):
        self.province = province

    def start_element(self, name, attrs):
        if name != 'map':
            name = attrs['title']
            number = attrs['href']
            self.province.append((name, number))
    def end_element(self, name):
        pass
    def char_data(self, text):
        pass

def get_province_entry(url):
     content = requests.get(url).content.decode('gb2312')
     start = content.find('<map name="map_86" id="map_86">')
     end = content.find('</map>')
     content = content[start:end + len('</map>')].strip()

     province = []
     handler = DefaultSaxHandler(province)
     parser = ParserCreate()
     parser.StartElementHandler = handler.start_element
     parser.EndElementHandler = handler.end_element
     parser.CharacterHandler = handler.char_data
     parser.parse(content)

     return province

province = get_province_entry('http://www.ip138.com/post')
print (province)
