# campus_partners_scrapers

## Stokes

Copy and paste the text of the webpage into an editor. Start each title with "T:" then run the script:

```
$ cat stokes.py
from datetime import datetime

with open("work.shops", "r") as f:
    lines = f.readlines()

def format_am_pm(x):
    if x.count("pm") == 2:
       return x.replace("pm", "") + " PM"
    if x.count("am") == 2:
       return x.replace("am", "") + " AM"
    return x

print("<html><head></head><body>")
for line in lines:
    if line.startswith("T:"):
        title = line[2:].strip()
    if line.startswith("Date & Time:"):
        date_time = line.replace("Date & Time:", "").strip()
        d = date_time.split("@")[0].strip()
        t = date_time.split("@")[1].strip()
        out = datetime.strptime(d, "%m/%d/%Y")
        date_time = out.strftime("%A, %B %d, %Y") + " at " + format_am_pm(t)
    if line.startswith("Instructors:"):
        instructors =  line.replace("Instructors", "Instructor").strip()
    if line.startswith("Reserve your spot:"):
        url =  line.replace("Reserve your spot:", "").strip()
    if line.startswith("Location:"):
        location = line
    if line.startswith("Description:"):
        print(f'<p><b><a href="{url}">{title}</a></b><br>')
        print(f"{date_time}<br>")
        print(f"{instructors}<br>")
        print(f"{location}<br></p>")
print("</body></html>")
```
