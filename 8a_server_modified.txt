"""Flask module for emotion detector"""
from flask import Flask,render_template,request
from EmotionDetection import emotion_detection

app = Flask(__name__)

@app.route('/')
def get_dashboard():
    """return the home page """
    return render_template('index.html')

@app.route('/emotionDetector')
def emotion_detector_req():
    """return the response of emotion detector input"""
    text_to_analyze = request.args.get('textToAnalyze')
    emotion_detected_obj = emotion_detection.emotion_detector(text_to_analyze)
    if emotion_detected_obj['dominant_emotion'] is None:
        return "Invalid text! Please try again!."
    response_txt = "the system response is "
    response_txt += f"'anger': {emotion_detected_obj['anger']}, "
    response_txt += f"'disgust': {emotion_detected_obj['disgust']}, "
    response_txt += f"'fear': {emotion_detected_obj['fear']}, "
    response_txt += f"'joy': {emotion_detected_obj['joy']} and "
    response_txt += f"'sadness': {emotion_detected_obj['sadness']}. "
    response_txt += f"The dominant emotion is {emotion_detected_obj['dominant_emotion']}."

    return response_txt

if __name__ == '__main__':
    app.run(debug = True)
