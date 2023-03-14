# 如何用 Flask 处理文件上传

> 原文：<https://blog.teclado.com/file-uploads-with-flask/>

在这篇文章中，让我告诉你如何允许你的用户向你的 Flask 应用程序发送文件。有简单的解决方案，也有更复杂的，看你的需求了！

今天我们来谈谈两种解决方案:一种适用于小文件，一种适用于大文件。

当 Flask 应用程序正在接收文件上传时，它不能在接收文件数据时做任何其他事情。如果文件非常大，这可能会完全阻止 Flask 应用程序，并阻止其他用户做任何事情。

对于小文件上传，我们将接受这一限制。对于大文件，我们会找到一种方法，这样当有人上传大文件时，应用程序的其他用户不会被锁定。

## 使用 Flask 轻松处理小文件上传

有多个 Flask 扩展来帮助我们上传文件。我发现开发最活跃的是[Flask-re-loaded](https://flask-reuploaded.readthedocs.io/en/latest/)。这是一个广泛使用的库 Flask-Uploads 的一个分支，该库已被弃用。

要在虚拟环境中安装库，请执行以下操作:

```
pip install flask-reuploaded 
```

然后，在你的 Flask 应用程序中，你必须创建一个`UploadSet`。为了创建它，我们需要一个名为的*和一个允许的文件扩展名列表。这个名字很重要，因为我们以后会用到它。*

```
import secrets  # built-in module
from flask import Flask, flash, render_template, request, redirect, url_for
from flask_uploads import IMAGES, UploadSet, configure_uploads

app = Flask(__name__)
photos = UploadSet("photos", IMAGES) 
```

然后，让我们在应用程序中添加一个密钥，并配置保存照片的文件夹。

`app.config["UPLOADED_PHOTOS_DEST"]`的名字中包含了`PHOTOS`，这是我们`UploadSet`的大写名字。所以如果你用`UploadSet("invoices", ["pdf"])`创建了你的`UploadSet`，那么配置应该是`UPLOADED_INVOICES_DEST`。文档中有更多信息。

调用 [`configure_uploads(app, photos)`](https://flask-reuploaded.readthedocs.io/en/latest/#app-configuration) 将每个`UploadSet`的配置存储在`app`中，这样 Flask 就可以访问了。

```
app.config["UPLOADED_PHOTOS_DEST"] = "static/img"
app.config["SECRET_KEY"] = str(secrets.SystemRandom().getrandbits(128))
configure_uploads(app, photos) 
```

最后，我们可以创建一个视图函数，接受来自`request.files`的文件，并使用存储在`photos`变量中的`UploadSet`实例来保存它:

```
@app.post("/upload")
def upload():
    if "photo" in request.files:
        photos.save(request.files["photo"])
        flash("Photo saved successfully.")
        return redirect(url_for("index")) 
```

我的`index` route 只是渲染一个显示文件上传表单的模板:

```
@app.get("/")
def index():
    return render_template("upload.html") 
```

这是`upload.html`模板:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flask-Reuploaded Example</title>
</head>
<body>
    {% with messages = get_flashed_messages() %}
        {% if messages %}
        <ul class="flashes">
        {% for message in messages %}
            <li>{{ message }}</li>
        {% endfor %}
        </ul>
        {% endif %}
    {% endwith %}

    <form method="POST" enctype="multipart/form-data" action="{{ url_for('upload') }}">
        <input type="file" name="photo">
        <button type="submit">Submit</button>
    </form>
</body>
</html> 
```

## 如何用 Flask 处理大文件上传

我们已经简要讨论了如何不能像发送小文件一样发送大文件，因为发送文件可能需要很长时间，并且会阻塞 Flask 应用程序。

相反，我们可以使用客户端库将文件分成块，然后一次上传一个。这样，Flask 应用程序可以在每个块之间为其他请求提供服务。

对于这一点，客户端库 [Dropzone.js](https://www.dropzone.dev/js/) 非常棒，并且易于使用！

当我们使用 Dropzone.js 上传文件时，它会将文件分割成块，一次上传一个。对于每个组块，还包括一些信息。在这一点上，Chris Griffith 的第一篇关于使用 Flask 库的文章获得了巨大的支持。这一段大量基于他的[原帖](https://codecalamity.com/uploading-large-files-by-chunking-featuring-python-flask-and-dropzone-js/)。

这是`request.form`在每个块上包含的内容:

*   `dzuuid`:标识文件的 UUID。
*   `dztotalfilesize`:整个文件的最终文件大小。
*   `dzchunkindex`:从 0 开始，每个块增加 1(例如，如果有 10 个块，从 0 到 9)。
*   `dztotalchunkcount`:预期的组块数(如 10)。
*   `dzchunksize`:每个组块的大小，虽然最后一个组块可能比这个小。
*   `dzchunkbyteoffset`:块大小*块索引，或开始写入数据的文件深度。

除此之外，`request.files`将包含块数据和上传文件名。

让我们从制作一个 Flask 应用程序来服务模板(将包括 Dropzone.js)和处理块开始:

```
import os
from pathlib import Path

from flask import Flask, render_template, request
from werkzeug.utils import secure_filename

app = Flask(__name__)

@app.get("/")
def index():
    return render_template("index.html")

@app.post("/upload")
def upload_chunk():
    pass 
```

这里是`index.html`。它:

*   包括 Dropzone.js JavaScript 和 CSS 文件。
*   用一个`id`属性定义一个 HTML `form`。
*   有一个 JavaScript 脚本在该表单上初始化 Dropzone.js，并确保它使用分块。

如果你想用它做更高级的事情，一定要阅读 Dropzone.js 文档，因为有很多[配置选项](https://docs.dropzone.dev/configuration/basics/configuration-options)可用。

```
<html lang="en">
<head>

    <meta charset="UTF-8">
    <script src="https://unpkg.com/[[email protected]](/cdn-cgi/l/email-protection)/dist/min/dropzone.min.js"></script>
    <link rel="stylesheet" href="https://unpkg.com/[[email protected]](/cdn-cgi/l/email-protection)/dist/min/dropzone.min.css" type="text/css" />

    <title>File Dropper</title>
</head>
<body>

    <form
        method="POST"
        action="/upload"
        class="dropzone dz-clickable"
        id="dropper"
        enctype="multipart/form-data"
    >
    </form>

    <script type="application/javascript">
        Dropzone.options.dropper = {
            paramName: "file",
            chunking: true,
            forceChunking: true,
            url: "/upload",
            maxFilesize: 1025, // megabytes
            chunkSize: 1000000 // bytes
        }
    </script>
</body>
</html> 
```

现在我们已经得到了这个，我们可以简单地上传一个文件，它将开始发送块到我们的`/upload`端点。让我们继续努力吧！

首先，让我们决定在哪里保存传入的图像。因为图像数据可能会分散到多个请求中，所以我们需要 Flask 知道请求中的文件名。我们可以使用上传文件名，这是用户计算机中的文件名，但这可能会导致问题。例如，用户尝试上传`photo.jpg`太常见了，然后我们需要实现错误处理，这样我们就不会覆盖现有的图像。

这就是`dzuuid`字段派上用场的地方。我们只需在上传文件名中添加几个字符，就可以得到一个唯一的文件名:

```
@app.post("/upload")
def upload_chunk():
    file = request.files["file"]
    file_uuid = request.form["dzuuid"]
    # Generate a unique filename to avoid overwriting using 8 chars of uuid before filename.
    filename = f"{file_uuid[:8]}_{secure_filename(file.filename)}" 
```

这样，我们现在可以想出一个保存图像的路径。假设我们想将它们保存到`static/img/FILENAME`。如果尚未创建`static/img`文件夹，请创建该文件夹:

```
@app.post("/upload")
def upload_chunk():
    file = request.files["file"]
    file_uuid = request.form["dzuuid"]
    # Generate a unique filename to avoid overwriting using 8 chars of uuid before filename.
    filename = f"{file_uuid[:8]}_{secure_filename(file.filename)}"
    save_path = Path("static", "img", filename) 
```

最后，开始攒块吧！我们所要做的就是在`save_path`打开文件，转到它的末尾，并写入我们收到的数据块。

要转到文件的结尾，我们将使用`dzchunkbyteoffset`,因为它应该告诉我们文件的结尾:

```
@app.post("/upload")
def upload_chunk():
    file = request.files["file"]
    filename = f"{uuid.uuid4().hex[:8]}_{secure_filename(file.filename)}"
    save_path = Path("static", "img", filename)

    with open(save_path, "ab") as f:
        f.seek(int(request.form["dzchunkbyteoffset"]))
        f.write(file.stream.read())

    return "Chunk upload successful.", 200 
```

瞧啊。你可以测试一下，你的文件上传应该可以了！

但是，没那么快...错误处理呢？

好吧，错误处理在博客文章中做得不好！比实际工作代码要长得多，复杂得多！

尽管如此，我们还是可以做一些事情。

首先，当我们保存完数据块后，我们可以检查最终的文件大小是否等于用户试图上传的文件大小。我们可以通过比较我们保存到`request.form["dztotalfilesize"]`的文件大小来做到这一点。我们还需要当前块和块的总数。

如果有错误，我们将用一条短消息和一个状态代码 500 来响应。这将显示在 Dropzone 表单中。

```
@app.post("/upload")
def upload_chunk():
    file = request.files["file"]
    file_uuid = request.form["dzuuid"]
    # Generate a unique filename to avoid overwriting using 8 chars of uuid before filename.
    filename = f"{file_uuid[:8]}_{secure_filename(file.filename)}"
    save_path = Path("static", "img", filename)
    current_chunk = int(request.form["dzchunkindex"])

    with open(save_path, "ab") as f:
        f.seek(int(request.form["dzchunkbyteoffset"]))
        f.write(file.stream.read())

    total_chunks = int(request.form["dztotalchunkcount"])

    # Add 1 since current_chunk is zero-indexed
    if current_chunk + 1 == total_chunks:
        # This was the last chunk, the file should be complete and the size we expect
        if os.path.getsize(save_path) != int(request.form["dztotalfilesize"]):
            return "Size mismatch.", 500

    return "Chunk upload successful.", 200 
```

好吧，还不算太糟！让我们再做一些。如果写入文件时出错怎么办。例如，文件已经存在，或者我们要保存文件的目录不存在。让我们在`open`文件时处理它:

```
@app.post("/upload")
def upload_chunk():
    file = request.files["file"]
    file_uuid = request.form["dzuuid"]
    # Generate a unique filename to avoid overwriting using 8 chars of uuid before filename.
    filename = f"{file_uuid[:8]}_{secure_filename(file.filename)}"
    save_path = Path("static", "img", filename)
    current_chunk = int(request.form["dzchunkindex"])

    try:
        with open(save_path, "ab") as f:
            f.seek(int(request.form["dzchunkbyteoffset"]))
            f.write(file.stream.read())
    except OSError:
        return "Error saving file.", 500

    total_chunks = int(request.form["dztotalchunkcount"])

    if current_chunk + 1 == total_chunks:
        # This was the last chunk, the file should be complete and the size we expect
        if os.path.getsize(save_path) != int(request.form["dztotalfilesize"]):
            return "Size mismatch.", 500

    return "Chunk upload successful.", 200 
```

你可以做的其他事情，Chris 在他最初的文章中做的是在上传的不同阶段添加日志。大多数人都忘记了日志记录，所以最好看看他的文章，看看他是如何处理的！

注意:Chris Griffith 在发表他的文章后，在[https://code hardisease . com/upload-large-files-fast-with-drop zone-js/](https://codecalamity.com/upload-large-files-fast-with-dropzone-js/)发表了一篇新的改进文章。这涉及到多线程，这又增加了问题的复杂性！

## 你应该在哪里存储 Flask 上传的文件？

存储文件是一个棘手的问题！文件非常大，至少与文本相比是如此，所以通常不希望将文件存储在数据库中。这将降低数据库的速度，并使其更难扩展。

相反，您的选择是:

*   将文件存储在运行您的应用程序的服务器中。
*   或者将文件上传到第三方文件存储服务，如亚马逊 S3 或 Backblaze B2。

### 在文件系统中存储文件

如果您的服务很简单，并且您使用单个服务器，那么将文件存储在服务器的文件系统中是可行的。这样做的好处是，至少在使用 Python 时，您所要做的就是我们在上面的代码示例中所做的。

写入文件，就大功告成了。

然后，当你希望你的应用程序使用该文件时，你只需访问磁盘中的文件即可。

简单！

但是有许多缺点...

*   如果您想扩展到 2 个以上的服务器，您需要找到不同的解决方案。为什么？因为每台服务器都有不同的文件，而且它们不能很容易地访问彼此的文件。
*   你必须自己处理备份，这样你才不会不小心丢失服务器里的所有东西。
*   服务器通常没有太多存储，因为大多数应用程序不需要太多存储。您最终可能会耗尽磁盘空间。
*   如果你要处理大量的文件上传和下载，你会发现硬盘会成为一个瓶颈。

因此，使用第三方服务来上传文件通常会更好。我一直在用 [Backblaze B2](https://www.backblaze.com/b2/cloud-storage.html) ，它非常容易使用。它的免费层也有 1TB 的存储空间。

如果您决定使用第三方文件存储服务，那么您的 Flask 应用程序将必须接受来自客户端的传入文件，将其保存到文件系统的临时文件夹中，并将其上传到第三方服务。这需要做更多的工作，但是你不需要在服务器中存储任何东西。我们已经写了一篇关于如何使用 Python 将文件上传到 Backblaze B2 的文章。

这意味着，如果你想启动更多的服务器来处理你的应用程序增加的流量，你可以不用担心。此外，文件存储服务将为您处理备份，并且无论您发出多少请求，都有足够的性能来处理您的请求。

当然，如果你需要比这些文件上传服务提供的免费计划更多的内容，你必须付费！这是主要的缺点！

如果你在建网站，使用 Flask，你可能会对我们的某个课程感兴趣，比如我们的 [Web Developer Bootcamp 和 Flask 与 Python](https://go.tecla.do/web-dev-course-sale) 。在这本书中，我们涵盖了网页设计、网页开发、Flask、数据库等等。点击链接以最优惠的价格参加课程！