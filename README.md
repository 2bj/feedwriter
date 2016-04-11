# feedwriter
Feedwriter port for Kohana

https://github.com/mibe/FeedWriter

## Example
```php
class News_Controller extends Controller {

    public function action_feed()
    {
        $Feed = new Feedwriter(FEEDWRITER_RSS2);
        $Feed->setTitle('News');
        $Feed->setLink('http://exmpale.com/news/rss');
        $Feed->setDescription('Super news');
        $Feed->setChannelElement('language', 'kg');
        $Feed->setChannelElement('pubDate', date(DATE_RSS, time()));
        
        $news_items = News_Model::feedItems();
        
        foreach ($news_items as $item){
            $link = 'http://example.com/news/' . $item['id'];
        
            $feed_item = $Feed->createNewItem();
            $feed_item->setTitle($item['title']);
            $feed_item->setLink($link . '?utm_source=rss&utm_medium=rss&utm_campaign=rss');
            $feed_item->setDate(strtotime($item['published_at']));
            $feed_item->setDescription($item['sub_title']);
            $feed_item->addElement('content:encoded', $item['content']);
            $feed_item->addElement('category', $item['rubric_name']);
            $feed_item->addElement('dc:creator', $item['author']);
            $feed_item->addElement('guid', $link, ['isPermaLink' => 'true']);
            
            $Feed->addItem($feed_item);
        }
        
        $Feed->generateFeed();
        die;
    }
}
```
