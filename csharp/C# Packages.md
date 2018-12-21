# C# Packages

## HtmlAgilityPack

```
			var hd=new HtmlAgilityPack.HtmlDocument();
			hd.LoadHtml(Clipboard.GetText());
			var nodes=hd.DocumentNode.SelectNodes("//li/a[contains(@class,'t-chapter')]")
				.Select(i=>i.InnerText.Trim())
				.Where(i=>char.IsNumber(i[0]))
				.Distinct();
```

## Markdig

- https://github.com/lunet-io/markdig

## Newtonsoft.Json

- https://github.com/JamesNK/Newtonsoft.Json

