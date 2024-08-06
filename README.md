import hashlib
import random
import string

class TinyURLService:
    def __init__(self):
        self.url_map = {}
        self.base_url = "http://tiny.url/"
        self.short_url_length = 6

    def _generate_short_code(self):
        """Generate a random short code."""
        characters = string.ascii_letters + string.digits
        short_code = ''.join(random.choice(characters) for _ in range(self.short_url_length))
        return short_code

    def shorten_url(self, long_url):
        """Shorten a long URL."""
        if long_url in self.url_map:
            return self.base_url + self.url_map[long_url]
        
        short_code = self._generate_short_code()
        while short_code in self.url_map.values():
            short_code = self._generate_short_code()
        
        self.url_map[long_url] = short_code
        return self.base_url + short_code

    def retrieve_url(self, short_url):
        """Retrieve the original long URL from a short URL."""
        short_code = short_url.replace(self.base_url, '')
        for long_url, code in self.url_map.items():
            if code == short_code:
                return long_url
        return None

if __name__ == "__main__":
    service = TinyURLService()
    
    long_url = "https://www.example.com/some/long/url"
    short_url = service.shorten_url(long_url)
    
    print("Short URL:", short_url)
    
    original_url = service.retrieve_url(short_url)
    print("Original URL:", original_url)

