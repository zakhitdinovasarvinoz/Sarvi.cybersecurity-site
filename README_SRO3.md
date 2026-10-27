Docker Hub: sarvinoz2006
Құралдар: Windows, PowerShell, Docker Desktop, Docker Compose, Python Flask, PostgreSQL
1. Жұмыстың мақсаты
Docker арқылы веб-қосымшаны контейнерде іске қосу, қосымша сервисті орналастыру, жеке жобаны баптау және Docker образдарын Docker Hub репозиторийіне жариялау.
2. Орындаған жобалар
   sarvinoz2006/moneytracker-web:1.0	MoneyTracker - кірістерді, шығыстарды және балансты есепке алатын Flask веб-қосымшасы.	Қолданылатын порт-http://localhost:8082
   Жоба / образ	Мақсаты	Қолданылатын порт
sarvinoz2006/moneytracker-web:1.0	MoneyTracker — кірістерді, шығыстарды және балансты есепке алатын Flask веб-қосымшасы	http://localhost:8082
sarvinoz2006/moneytracker-db:1.0	PostgreSQL дерекқор жүйесінің жеке Docker Hub-қа көшірілген образы.Қолданылатын порт- Веб-қосымша ішінде db:5432
sarvinoz2006/system-monitor:1.0	System Monitor — CPU, RAM және уақыт көрсеткіштері бар Flask сайты.Қолданылатын порт-http://localhost:8083
Ескерту: PostgreSQL образы - дайын ресми postgres:17-alpine образының жеке тегпен жарияланған көшірмесі. MoneyTracker қолданбасы іске қосылғанда Compose конфигурациясы сол ресми образды (postgres:17-alpine) қолдана алады. Образ бен контейнер бір нәрсе емес.
3. Жобаның құрылымы
MoneyTracker:
moneytracker/
├── app.py
├── Dockerfile
├── compose.yaml
├── requirements.txt
├── .env.example
├── README.md
├── templates/
│   └── index.html
└── static/
    └── style.css
   System Monitor:
   SystemMonitor/
├── app.py
├── Dockerfile
└── requirements.txt

4. MoneyTracker жобасын бастапқы кодтан іске қосу (PowerShell)
Алдымен Docker Desktop іске қосылғанына көз жеткізіңіз. MoneyTracker жобасының compose.yaml орналасқан қалтасын ашыңыз.
cd "$HOME\Desktop\MoneyTrackerProject\moneytracker"
Copy-Item .env.example .env
notepad .env
.env ішіне Docker Hub пайдаланушысының атын және өз құпия мәндеріңізді енгізіңіз:
DOCKERHUB_USER=sarvinoz2006
POSTGRES_PASSWORD=осында_кездейсоқ_күрделі_құпиясөз
SECRET_KEY=осында_басқа_кездейсоқ_құпия_кілт

Нақты парольді README файлына, GitHub-қа немесе Docker Hub-қа жарияламаңыз. compose.yaml ішіндегі web сервисінің порттары мынадай болуы тиіс (сайт контейнерде 5000 портын тыңдайды):
ports:
  - "127.0.0.1:8082:5000"

    Браузерде: http://localhost:8082
Денсаулық тексерісі (жобада /health маршруты бар болса):
curl.exe http://localhost:8082/health
Сайтта бір кіріс пен бір шығыс қосып, баланс пен операциялар тізімін көрсетіңіз.
PostgreSQL тексеру
docker compose exec db psql -U moneyuser -d moneytracker
psql ішінде кестелер тізімін көрсету:
\dt
Кесте атауы мен бағандарды нақты жобадан тексеріп, содан кейін деректерді оқыңыз. Мысалы, кесте transaction деп аталса:
SELECT * FROM "transaction" LIMIT 10;
Шығу: \q.

Дерекқор деректері Docker volume ішінде сақталады. docker compose down әдетте volume-ды жоймайды; ал docker compose down -v деректерді жоюы мүмкін, сондықтан оны қолданбаңыз.
6. System Monitor жобасын іске қосу
Жоба қалтасында:
cd "$HOME\Desktop\SystemMonitor"
docker build -t sarvinoz2006/system-monitor:1.0 .
docker run -d --name system-monitor -p 127.0.0.1:8083:5000 sarvinoz2006/system-monitor:1.0
docker ps
Браузерде: http://localhost:8083.

Контейнер бұрыннан жасалған болса, docker run орнына docker start system-monitor командасын орындаңыз. Қателерді қарау:
docker logs system-monitor
Маңызды: System Monitor ішіндегі CPU және RAM деректері контейнерге көрінетін ортаны сипаттайды; бұл Windows компьютерінің барлық көрсеткіштеріне дәл сәйкес келмеуі мүмкін.
7. Docker Hub-қа жариялау
docker login
docker push sarvinoz2006/moneytracker-web:1.0
docker push sarvinoz2006/moneytracker-db:1.0
docker push sarvinoz2006/system-monitor:1.0
Жарияланған образдарды тексеру:
- https://hub.docker.com/r/sarvinoz2006/moneytracker-web
- https://hub.docker.com/r/sarvinoz2006/moneytracker-db
- https://hub.docker.com/r/sarvinoz2006/system-monitor
