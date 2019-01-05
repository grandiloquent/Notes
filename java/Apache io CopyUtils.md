# Java: Apache io CopyUtils

```java

package org.apache.commons.io;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.io.StringReader;
import java.io.Writer;
import java.nio.charset.Charset;
@Deprecated
public class CopyUtils {

    private static final int DEFAULT_BUFFER_SIZE = 1024 * 4;

    public CopyUtils() { }




    public static void copy(final byte[] input, final OutputStream output)
            throws IOException {
        output.write(input);
    }




    @Deprecated
    public static void copy(final byte[] input, final Writer output)
            throws IOException {
        final ByteArrayInputStream in = new ByteArrayInputStream(input);
        copy(in, output);
    }

    public static void copy(
            final byte[] input,
            final Writer output,
            final String encoding)
                throws IOException {
        final ByteArrayInputStream in = new ByteArrayInputStream(input);
        copy(in, output, encoding);
    }




    public static int copy(
            final InputStream input,
            final OutputStream output)
                throws IOException {
        final byte[] buffer = new byte[DEFAULT_BUFFER_SIZE];
        int count = 0;
        int n = 0;
        while (-1 != (n = input.read(buffer))) {
            output.write(buffer, 0, n);
            count += n;
        }
        return count;
    }




    public static int copy(
            final Reader input,
            final Writer output)
                throws IOException {
        final char[] buffer = new char[DEFAULT_BUFFER_SIZE];
        int count = 0;
        int n = 0;
        while (-1 != (n = input.read(buffer))) {
            output.write(buffer, 0, n);
            count += n;
        }
        return count;
    }




    @Deprecated
    public static void copy(
            final InputStream input,
            final Writer output)
                throws IOException {

        final InputStreamReader in = new InputStreamReader(input, Charset.defaultCharset());
        copy(in, output);
    }

    public static void copy(
            final InputStream input,
            final Writer output,
            final String encoding)
                throws IOException {
        final InputStreamReader in = new InputStreamReader(input, encoding);
        copy(in, output);
    }




    @Deprecated
    public static void copy(
            final Reader input,
            final OutputStream output)
                throws IOException {

        final OutputStreamWriter out = new OutputStreamWriter(output, Charset.defaultCharset());
        copy(input, out);


        out.flush();
    }

    public static void copy(
            final Reader input,
            final OutputStream output,
            final String encoding)
                throws IOException {
        final OutputStreamWriter out = new OutputStreamWriter(output, encoding);
        copy(input, out);


        out.flush();
    }




    @Deprecated
    public static void copy(
            final String input,
            final OutputStream output)
                throws IOException {
        final StringReader in = new StringReader(input);

        final OutputStreamWriter out = new OutputStreamWriter(output, Charset.defaultCharset());
        copy(in, out);


        out.flush();
    }

    public static void copy(
            final String input,
            final OutputStream output,
            final String encoding)
                throws IOException {
        final StringReader in = new StringReader(input);
        final OutputStreamWriter out = new OutputStreamWriter(output, encoding);
        copy(in, out);


        out.flush();
    }




    public static void copy(final String input, final Writer output)
                throws IOException {
        output.write(input);
    }
}
```

