## Wordpress

* [Wordpress migration with a different hostname](#wordpress-migration-with-a-different-hostname)

### Wordpress migration with a different hostname

```
UPDATE wp_options SET option_value = replace(option_value, 'PREVIOUS_URL', 'NEW_URL') WHERE option_name = 'home' OR option_name = 'siteurl';
UPDATE wp_posts SET guid = replace(guid, 'PREVIOUS_URL','NEW_URL');
UPDATE wp_posts SET post_content = replace(post_content, 'PREVIOUS_URL', 'NEW_URL');
UPDATE wp_postmeta SET meta_value = replace(meta_value,'PREVIOUS_URL','NEW_URL');

-- IF USING EDGE PLUGIN :

UPDATE wp_options SET option_value = replace(option_value, 'PREVIOUS_URL', 'NEW_URL')
```




