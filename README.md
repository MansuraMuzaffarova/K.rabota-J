# K.rabota-J
import cv2
import numpy as np
from PIL import Image, ImageTk
from sklearn.svm import SVC
import joblib
import tkinter as tk

class CarDetector:
    def _init_(self):
        self.root = tk.Tk()
        self.root.title("Car Detector")

        self.cap = cv2.VideoCapture(0)

        self.video_label = tk.Label(self.root)
        self.video_label.pack()

        self.alert_window = tk.Toplevel(self.root)
        self.alert_window.title("Alert Window")

        self.canvas = tk.Canvas(self.alert_window, width=200, height=200, bg="red")
        self.car_rect = self.canvas.create_rectangle(50, 50, 150, 150, fill="black")
        self.canvas.pack()

        self.car_detected = False

        self.loaded_svm_model = joblib.load('svm_model.pkl')

        self.update()

        self.root.protocol("WM_DELETE_WINDOW", self.on_closing)

    def update(self):
        ret, frame = self.cap.read()

        rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)

        img = Image.fromarray(rgb_frame)
        img = ImageTk.PhotoImage(image=img)

        self.video_label.img = img
        self.video_label.config(image=img)

        cars = self.detect_cars(frame)

        if cars == 1:
            if not self.car_detected:
                self.alert_window.deiconify()
                self.car_detected = True
        else:
            self.alert_window.withdraw()
            self.car_detected = False

        self.root.after(10, self.update)

    def detect_cars(self, frame):
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

        img = gray.flatten()

        prediction = self.loaded_svm_model.predict([img])

        return prediction

    def on_closing(self):
        self.cap.release()
        self.root.destroy()

if _name_ == "_main_":
    car_detector = CarDetector()
    car_detector.root.mainloop()


import cv2
import numpy as np
from PIL import Image, ImageTk
from sklearn.svm import SVC
import joblib
import tkinter as tk

class CarDetector:
    def _init_(self):
        self.root = tk.Tk()
        self.root.title("Car Detector")

        self.cap = cv2.VideoCapture(0)

        self.video_label = tk.Label(self.root)
        self.video_label.pack()

        self.alert_window = tk.Toplevel(self.root)
        self.alert_window.title("Alert Window")

        self.canvas = tk.Canvas(self.alert_window, width=200, height=200, bg="red")
        self.car_rect = self.canvas.create_rectangle(50, 50, 150, 150, fill="black")
        self.canvas.pack()

        self.car_detected = False

        self.loaded_svm_model = joblib.load('svm_model.pkl')

        self.update()

        self.root.protocol("WM_DELETE_WINDOW", self.on_closing)

    def update(self):
        ret, frame = self.cap.read()

        rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)

        img = Image.fromarray(rgb_frame)
        img = ImageTk.PhotoImage(image=img)

        self.video_label.img = img
        self.video_label.config(image=img)

        cars = self.detect_cars(frame)

        if cars == 1:
            if not self.car_detected:
                self.alert_window.deiconify()
                self.car_detected = True
        else:
            self.alert_window.withdraw()
            self.car_detected = False

        self.root.after(10, self.update)

    def detect_cars(self, frame):
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

        img = gray.flatten()

        prediction = self.loaded_svm_model.predict([img])

        return prediction

    def on_closing(self):
        self.cap.release()
        self.root.destroy()

if _name_ == "_main_":
    car_detector = CarDetector()
    car_detector.root.mainloop()
