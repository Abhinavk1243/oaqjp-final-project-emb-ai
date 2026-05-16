from flask import Flask,render_template,request,make_response
from EmotionDetection import emotion_detection

app = Flask(__name__)

@app.route('/')
def get_dashboard():
    return render_template('index.html')

@app.route('/emotionDetector')
def emotion_detector_req():

    text_to_analyze = request.args.get('textToAnalyze')
    emotion_detected_obj = emotion_detection.emotion_detector(text_to_analyze)
    if emotion_detected_obj['dominant_emotion'] is None:
        return "Invalid text! Please try again!."
    response_txt = f"the system response is 'anger': {emotion_detected_obj['anger']}, 'disgust': {emotion_detected_obj['disgust']}, 'fear': {emotion_detected_obj['fear']}, 'joy': {emotion_detected_obj['joy']} and 'sadness': {emotion_detected_obj['sadness']}. The dominant emotion is {emotion_detected_obj['dominant_emotion']}." 
    return response_txt

if __name__ == '__main__':
    app.run(debug = True)