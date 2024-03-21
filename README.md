import cv2
import tkinter as tk
from tkinter import filedialog

# Add watermark function
def add_watermark(video_path, text):
    cap = cv2.VideoCapture(video_path)
    width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
    height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
    fourcc = cv2.VideoWriter_fourcc(*'XVID')
    out = cv2.VideoWriter('output_video.avi', fourcc, 20.0, (width, height))

    while cap.isOpened():
        ret, frame = cap.read()
        if not ret:
            break

        # Add watermark text
        cv2.putText(frame, text, (10, 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 255, 255), 2)

        out.write(frame)

    cap.release()
    out.release()
    cv2.destroyAllWindows()

# Display GUI
def create_gui():
    root = tk.Tk()
    root.title("Add your watermark")
    root.geometry("500x500")
    root.configure(bg='green')

    def browse_file():
        file_path = filedialog.askopenfilename()
        add_watermark(file_path, entry_text.get())

    label = tk.Label(root, text="Please add your text for watermark:")
    label.pack()

    entry_text = tk.Entry(root)
    entry_text.pack()

    button_browse = tk.Button(root, text="Choose video", command=browse_file)
    button_browse.pack()

    root.mainloop()

if __name__ == "__main__":
    create_gui()
