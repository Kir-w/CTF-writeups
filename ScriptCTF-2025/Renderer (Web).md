> Introducing Renderer! A free-to-use app to render your images!
> 
> Attachments
> chall.zip, including app.py, flag.txt, static\uploads\secrets\secret_cookie.txt, templates\display.html, templates\upload.html
> 
> Instance

<details>
<summary>app.py</summary>

    from flask import Flask, request, redirect, render_template, make_response, url_for
    
    app = Flask(__name__)
    
    from hashlib import sha256
    
    import os
    
    def allowed(name):
        if name.split('.')[1] in ['jpg','jpeg','png','svg']:
            return True
        return False
    
    @app.route('/',methods=['GET','POST'])
    
    def upload():
        if request.method == 'POST':
            if 'file' not in request.files:
                return redirect(request.url)
            file = request.files['file']
            if file.filename == '':
                return redirect(request.url)
            if file and allowed(file.filename):
                filename = file.filename
                hash = sha256(os.urandom(32)).hexdigest()
                filepath = f'./static/uploads/{hash}.{filename.split(".")[1]}'
                file.save(filepath)
                return redirect(f'/render/{hash}.{filename.split(".")[1]}')
        return render_template('upload.html')
    
    @app.route('/render/<path:filename>')
    
    def render(filename):
        return render_template('display.html', filename=filename)
    
    @app.route('/developer')
    
    def developer():
        cookie = request.cookies.get("developer_secret_cookie")
        correct = open('./static/uploads/secrets/secret_cookie.txt').read()
        if correct == '':
            c = open('./static/uploads/secrets/secret_cookie.txt','w')
            c.write(sha256(os.urandom(16)).hexdigest())
            c.close()
        correct = open('./static/uploads/secrets/secret_cookie.txt').read()
        if cookie == correct:
            c = open('./static/uploads/secrets/secret_cookie.txt','w')
            c.write(sha256(os.urandom(16)).hexdigest())
            c.close()
            return f"Welcome! There is currently 1 unread message: {open('flag.txt').read()}"
        else:
            return "You are not a developer!"
    
    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=1337) 

</details>

flag.txt contains scriptCTF{fakeflag}.

Se rendre à l'URL suivante : http://play.scriptsorcerers.xyz:10101/render/../static/uploads/secrets/secret_cookie.txt.

Le serveur va retourner le contenu du fichier secret_cookie.txt qui est la valeur du cookie secret : renvoie vers http://play.scriptsorcerers.xyz:10101/static/uploads/secrets/secret_cookie.txt en affichant une page blanche et vide.

http://play.scriptsorcerers.xyz:10073/developer
puis http://play.scriptsorcerers.xyz:10073/static/uploads/secrets/secret_cookie.txt avec : 
30d33dcefb611c007679dc15ca3654e2c3ac202bc73dad4e69b00c6a3683758a

Puis changer le session cookie en developer_secret_cookie avec la valeur précédente et actualiser.

<details>
<summary>Flag</summary>

`scriptCTF{my_c00k135_4r3_n0t_s4f3!_bc28cb55ffa3}`

</details>
