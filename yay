app = Flask(__name__)
nlp = spacy.load('en_core_web_sm')

def transform_resume(resume_text):
 doc = nlp(resume_text)
 transformed_tokens = []
 for token in doc:
 if token.is_alpha:
 pos = pos_tag([token.text])[0][1]
 if pos.startswith('NN'):
 lemma = token.lemma_.lower()
 synsets = wordnet.synsets(lemma)
 if synsets:
 gender = synsets[0].lexname().split('.')[-2]
 if gender in ['male', 'female']:
 gender_neutral_lemma = 'person'
 transformed_tokens.append(gender_neutral_lemma)
 else:
 transformed_tokens.append(token.text)
 else:
 transformed_tokens.append(token.text)
 return ' '.join(transformed_tokens)

@app.route('/', methods=['GET', 'POST'])
def index():
 if request.method == 'POST':
 resume_text = request.form['resume']
 transformed_resume = transform_resume(resume_text)
 return render_template('result.html', transformed_resume=transformed_resume)
 return render_template('index.html')

if __name__ == '__main__':
 app.run(debug=True)

<!DOCTYPE html>
<html>
<head>
 <title>Gender-Neutral Resume</title>
 <link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='styles.css') }}">
</head>
<body>
 <div class="container">
 <h1>Transform Your Resume</h1>
 <form method="post">
 <textarea name="resume" rows="10" placeholder="Paste your resume here..."></textarea><br>
 <input type="submit" value="Transform">
 </form>
 </div>
</body>
</html>

templates/result.html:
html
<!DOCTYPE html>
<html>
<head>
 <title>Transformed Resume</title>
 <link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='styles.css') }}">
</head>
<body>
 <div class="container">
 <h1>Transformed Resume</h1>
 <p>{{ transformed_resume }}</p>
 <a href="/">Transform Another Resume</a>
 </div>
</body>
</html>
