# uro
Using a URL list for security testing can be painful as there are a lot of URLs that have uninteresting/duplicate content; **uro** aims to solve that.

It doesn't make any http requests to the URLs and removes:
- incremental urls e.g. `/page/1/` and `/page/2/`
- blog posts and similar human written content e.g. `/posts/a-brief-history-of-time`
- urls with same path but parameter value difference e.g. `/page.php?id=1` and `/page.php?id=2`
- images, js, css and other "useless" files

#### Installation
The recommended way to install uro is through pip as follows:
```
pip3 install uro --user
```
### Basic Usage
```
cat urls.txt | uro
```

![uro-demo](https://i.ibb.co/x2tWCC5/uro-demo.png)

### Advanced usage
#### Reading urls from a file (-i/--input)

`python3 uro.txt -i input.txt`

#### Writing urls to a file (-o/--output)
If the file already exists, uro will not overwrite the contents. Otherwise, it will create a new file.

`python3 uro.txt -i input.txt -o output.txt`

#### Whitelist (`-w/--whitelist`)
uro will ignore all other extension except the ones provided.

`uro -w php asp html`

**Note:** Extensionless pages e.g. /books/1 will still be included. To remove them too, use  `--filter hasext`.

#### Blacklist (`-b/--blacklist`)
uro will ignore the given extensions.

`uro -b jpg png js pdf`

**Note:** uro has a list of "useless" extensions which it removes by default; that list will be overidden by whatever extensions you provide through blacklist option. Extensionless pages e.g. /books/1 will still be included. To remove them too, use `--filter hasext`.

#### Filters (-f/--filters)
For granular control, uro supports the following filters:

1. **hasparams:** only output urls that have query parameters e.g. `http://example.com/page.php?id=`
2. **noparams:** only output urls that have no query parameters e.g. `http://example.com/page.php`
3. **hasexts:** only output urls that have extensions e.g. `http://example.com/page.php`
4. **noexts:** only output urls that have no extensions e.g. `http://example.com/page`
5. **keepcontent:** keep human written content e.g. blogs.
6. **keepslash:** don't remove trailing slash from urls e.g. `http://example.com/page/`

Example: `uro --filters hasexts hasparams`
