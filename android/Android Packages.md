# Android Packages

## Android Source Code

- [Platform Frameworks Base](https://github.com/aosp-mirror/platform_frameworks_base)
- [bionic](https://github.com/aosp-mirror/platform_bionic)

## Glide

- https://github.com/bumptech/glide

## jsoup

```kotlin
                val js = Jsoup.parse(it)
                val elements = js.select(".b-pager-pages option")

                if (elements.size >= 0) {
                    val count = elements.size;
                    for (index in 2..count) {
                        val url = "$url?page=$index"

                    }
                }
```

```kotlin
fun String.htm2txt(special: Boolean = false): String {

    val d = Jsoup.parse(this)
    val f = FormattingVisitor()
    if (special) {
        NodeTraversor(f).traverse(d?.body()?.select(".b-story-body-x")?.first())
        return (d?.body()?.select(".b-story-header h1")?.first()?.text()
                ?: "") + "\n" + f.toString()
    } else {
        NodeTraversor(f).traverse(d?.body())
        return f.toString()
    }

}
```

```kotlin
class FormattingVisitor : NodeVisitor {
    override fun tail(node: Node?, depth: Int) {
        if (mT2.contains(node?.nodeName())) {
            mSb.append('\n')
        }
    }

    override fun head(node: Node?, depth: Int) {

        if (node is TextNode) {
            mSb.append(node.text())
        } else if (node?.nodeName() == "li") {
            mSb.append("\n * ");
        } else if (node?.nodeName() == "dt") {
            mSb.append(" ")
        } else if (mT1.contains(node?.nodeName())) {
            mSb.append("\n")
        }

    }

    override fun toString(): String {
        return mSb.toString()
    }

    private val mT1 = arrayOf("p", "h1", "h2", "h3", "h4", "h5", "tr")
    private val mT2 = arrayOf("br", "dd", "dt", "p", "h1", "h2", "h3", "h4", "h5")
    private val mSb = StringBuilder()

}
```

- https://github.com/jhy/jsoup

## LeakCanary

- https://github.com/square/leakcanary

## OkHttp

```kotlin
private const val USER_AGENT = "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.90 Safari/537.36"
private const val ACCEPT = "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8"

fun String.fetchHtml(): String? {
    val oh = OkHttpClient
            .Builder()
            .connectTimeout(15, TimeUnit.SECONDS)
            .readTimeout(15, TimeUnit.SECONDS)
            .build()
    val req = Request.Builder()
            .url(this)
            .addHeader("accept", ACCEPT)
            .addHeader("user-agent", USER_AGENT)
            .build()
    val res = oh.newCall(req).execute()
    if (res.isSuccessful) {
        return res.body()?.string()
    }
    return null;
}
```

- https://github.com/square/okhttp/wiki/Recipes
- https://github.com/square/okhttp

