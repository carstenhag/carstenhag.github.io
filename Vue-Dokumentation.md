# Vue-Planner

# Was ist Vue.js?
Vue.js ist ein JavaScript-Framework für das Erstellen von modernen Web-Anwendungen. Es ist vergleichbar mit Angular und React.

Vue.js selbst beinhaltet nur Anzeige-Funktionalitäten zum Erstellen von Oberflächen. Weitere Funktionalitäten wie Daten-Persistierung, Datenabfragen oder Router-Handling können mit 3rd-Party-Libraries eingebunden werden, zum Beispiel mit `vue-resource` oder `vue-router`.

React und Vue sind weitestgehend vergleichbar. In React wird die ganze Web-Anwendung in JavaSript bzw. einer erweiterten Syntax namens JSX.  
React besitzt eine höhere Lernkurve, da in den Beispielen und in den generierten Projekten der Command-Line-Anwendung JSX und die moderne ES2015+-JavaScript-Syntax benutzt wird, die es erst seit wenigen Jahren gibt und Einarbeitung erfordert.

Angular erfordert, so wie Vue, eine höhere Einarbeitung, da die API generell komplizierter ist und Angular das Verständnis über verschiedene Konzepte erfordert, bis effektiv entwickelt werden kann.
Angular erfordert die Benutzung von TypeScript, eine JavaScript-ähnliche Sprache die zu JavaScript transpiliert wird. Sie bringt Typ-Informationen mit sich. In Vue ist die Benutzung von TypeScript optional, aber auch Verfügbar. Auch TypeScript erfordert ein Umdenken und eine Einarbeitung.

# Das Team
Carsten Hagemann, Felix Bertsch, Niklas Portmann, Jannik Zeyer

# Das Projekt
Für die Studierenden der DHBW Mosbach soll eine Anwendung entwickelt werden, die Vorlesungen, News und Events der Hochschule und der Studierendenvertretung Mosbach anzeigt. Es besteht bereits eine Android-App und eine iOS-App. Diese wurden aber seit mehr als einem Jahr nicht mehr weiterentwickelt und der iOS-App fehlt die Anzeige der News und der Events.

Als plattformübergreifende Lösung wurde dieses Projekt nun in JavaScript, mit dem Vue.js Framework, umgesetzt. Die Web-Anwendung soll über einer Webseite, über einer Android-App und einer iOS-App verfügbar sein, damit alle Studierende die Infos auf ihren bevorzugten Geräten ansehen können.

## Benutzte API
Es werden drei Programmierschnittstellen benutzt. Diese geben unterschiedliche Datentypen zurück. Es folgt eine Erläuterung der verschiedenen API:

###  DHBW Kalenderserver
Die DHBW Mosbach besitzt einen Kalenderserver in dem die Vorlesungen eingetragen werden. Die API-URL hierzu wurden in der bereits existierenden Android-App gefunden.

Die Liste mit den Kursen und den jeweiligen Kalenderdateien für die Vorlesungen ist unter `http://ics.mosbach.dhbw.de/ics/calendars.list` verfügbar. Die zurückgegebene Liste ist eine Semikolon-separierte Textdatei (analog zu CSV). Einige Einträge sind ungültig und müssen bereinigt werden:

```
[...]
ET15A;http://ics.mosbach.dhbw.de/ics/et15a.ics
ON15B;http://ics.mosbach.dhbw.de/ics/on15b.ics
;http://ics.mosbach.dhbw.de/ics/.
INF16B;http://ics.mosbach.dhbw.de/ics/inf16b.ics
CALENDARS;http://ics.mosbach.dhbw.de/ics/calendars.list
IN15C;http://ics.mosbach.dhbw.de/ics/in15c.ics
```

Die folgende Funktion erstellt ein JavaScript-Objekt, in dem die Einträge als Key-Value-Pair gespeichert werden. Außerdem werden die Fehleinträge bereinigt:

```js
const data = response.body // response.body is the API return value
const lines = data.split('\n')
let courseList = {}
for (let line of lines) {
    let lineSplit = line.split(';') // Gibt ein Array zurück
    if (lineSplit[0].length !== 0 && lineSplit !== 'CALENDARS') {
        courseList[lineSplit[0]] = lineSplit[1]
    }
}
```

Die URL für die jeweiligen Kurse haben wir nun also in eine Variable gespeichert. Der Benutzer der Anwendung kriegt über ein `<select>`-Element die Liste von verfügbaren Kurse angezeigt. Nach dem Klick auf einem Kurs wird die zugehörige `.ics`-Datei aufgerufen und in einer Variable gespeichert.

```js
parseCalendar() {
    let URL = this.getCourseList[this.selectedCourse]
    this.isUpdating = true

    this.$http.get(URL).then(response => {
        const icsData = response.body
        
        const parsedLectures = Ical.parse(icsData)[2].slice(1, -1) // not sure if also applies to other curses
        this.$store.commit('updateLectures', parsedLectures)

        this.$store.commit('updateGroupedLectures', this.groupLecturesByDate)
        this.isUpdating = false
        this.parseCourseList()
    }, response => {
        this.isUpdating = false
        console.log(response)
    })
}
```

Danach wird die [ical.js](https://github.com/mozilla-comm/ical.js/)-Bibliothek von Mozilla benutzt, um die Events von dem iCalendar-Format zu JSON umzuwandeln.

Ein Beispiel-Eintrag des iCalendar-Formats folgt. iCalendar-Dateien sind Textbasiert, aber sind nicht wirklich strukturiert:

```ics
[...]
BEGIN:VEVENT
UID:DHBW_on15b2018060509:00
DTSTAMP:20180203T204604Z
DTSTART;TZID=Europe/Berlin:20180605T090000
CLASS:PUBLIC
CREATED:20180203T204604Z
DESCRIPTION:U.Andersson
LAST-MODIFIED:20180203T204604Z
LOCATION:D-0.210
SUMMARY:Multimedia 3 Vorlesung
DTEND;TZID=Europe/Berlin:20180605T143000
END:VEVENT
[...]
```



### Google Calendar API
Die [Google Calendar API](https://developers.google.com/calendar/) wird benutzt um die Events und Termine der Studierendenvertretung Mosbach zu bekommen. Die Studierendenvertretung trägt die Termine über der Google Calendar-Webseite ein, und über der API bekommen wir jederzeit die aktuellen Termine.

Die API-URL inklusive API-Key ist `https://www.googleapis.com/calendar/v3/calendars/asta.dhbw.de_08mkcuqcrppq8cg8vlutdsgpjg@group.calendar.google.com/events?key=AIzaSyC6mAcPZYgvYcWW_2w5e3Oy7LOHfj-wTMM`.

Die folgenden Codeblöcke zeigen, wie die API abgefragt wird, und wie die Events gefiltert werden.
```js
parseCalendar() {
    let URL = 'https://www.googleapis.com/calendar/v3/calendars/asta.dhbw.de_08mkcuqcrppq8cg8vlutdsgpjg@group.calendar.google.com/events'
    URL += '?key=AIzaSyC6mAcPZYgvYcWW_2w5e3Oy7LOHfj-wTMM'

    this.$http.get(URL).then(response => {
        const data = response.body
        let events = data.items

        events = this.filterOldEvents(events)
        this.$store.commit('updateEvents', events)
    }, response => {
        console.log(response)
    })
}
```

In `filterOldEvents` werden Events (Termineinträge) aus der API-Rückgabe entfernt, deren Ablaufdatum mehr als 7 Tage alt ist. So werden nur bevorstehende, aktuelle oder vor kurzem beendete Termine/Events angezeigt.

Ohne dieser Filterung würden ca 300 Einträge angezeigt werden, die schon 3-4 Jahre alt sind.
```js
filterOldEvents(events) {
    let filteredEvents = []
    for (let event of events) {
        // Filter out events with weird timestamp and events older than 7 days
        // refactor to make prettier
        if (event.start.dateTime !== undefined && moment().subtract(7, 'days').isSameOrBefore(event.start.dateTime, 'day') ||
            (event.start.date !== undefined && moment().subtract(7, 'days').isSameOrBefore(event.start.date, 'day'))) {
            filteredEvents.push(event)
        }
    }
    return filteredEvents
}
```

Ein einzelnes Event von der Google Calendar-API ist wie folgt strukturiert. Die wichtigen Daten für unsere Anwendung sind die Variablen `summary`, `description`, `location` und `start`. Aus diesen Daten werden später die Datumsangaben, Titel, Ort und Beschreibung generiert.
```json
{
    kind: "calendar#event",
    etag: "3039655544560000",
    id: "6bs5garplfec98p6u6tfa23sg4",
    status: "confirmed",
    htmlLink: "https://www.google.com/calendar/event?eid=NmJzNWdhcnBsZmVjOThwNnU2dGZhMjNzZzQgYXN0YS5kaGJ3LmRlXzA4bWtjdXFjcnBwcThjZzh2bHV0ZHNncGpnQGc",
    created: "2018-02-28T12:36:19.000Z",
    updated: "2018-02-28T14:22:52.280Z",
    summary: "Karaoke",
    description: "An alle Rockstars und Unter-der-Dusche-Sänger: Es ist wieder so weit, wir feiern wieder StuV-Karaoke. Wir treffen uns am 10.04 um 18:00 Uhr in der Tante Gerda, Carl-Theodor-Straße 10. Bei über 25.000 Songs gibt es alles von A, wie Alphaville, bis Z, wie ZZ Top!Ihr wolltet euren Kommilitonen zeigen, dass ihr fast den Sprung auf die ganz große Bühne geschafft hättet. Dann kommt vorbei! Wir freuen uns, Eure StuV",
    location: "Tante Gerda, Carl-Theodor-Straße 10, 74821 Mosbach, Deutschland",
    creator: {
        email: "j.franz@stuv-mosbach.de"
    },
    organizer: {
        email: "asta.dhbw.de_08mkcuqcrppq8cg8vlutdsgpjg@group.calendar.google.com",
        displayName: "Events StuV Mosbach (Public)",
        self: true
    },
    start: {
        dateTime: "2018-03-06T18:00:00+01:00"
    },
    end: {
        dateTime: "2018-03-06T23:30:00+01:00"
    },
    iCalUID: "6bs5garplfec98p6u6tfa23sg4@google.com",
    sequence: 0
},
```

### WordPress API
Wir benutzen die [Wordpress Posts API](https://developer.wordpress.org/rest-api/reference/posts/) um die letzten 10 Newsbeiträge der Studierendenvertretung Mosbach zu bekommen.

Die API-URL ist `https://stuv-mosbach.de/wp-json/wp/v2/posts?per_page=10`.

```json
{  
	"id": 1194,  
	"date": "2017-04-20T11:08:28",  
	"date_gmt": "2017-04-20T09:08:28",  
	"guid": {  
	  "rendered": "https:\\/\\/stuv-mosbach.de\\/?p=1194"  
	},  
	"modified": "2017-04-20T11:08:28",  
	"modified_gmt": "2017-04-20T09:08:28",  
	"slug": "kino6",  
	"status": "publish",  
	"type": "post",  
	"link": "https:\\/\\/stuv-mosbach.de\\/2017\\/04\\/kino6\\/",  
	"title": {  
	  "rendered": "Kino unter den Sternen"  
	},  
	"content": {  
	  "rendered": [omitted, example below],  
	  "protected":false  
	},
	...
}
```

Ein Post-Inhalt (in der JSON-Antwort `content.rendered`) wäre zum Beispiel:

`<p>Am 4. Mai findet in der Mosbacher Innenstadt das Kino unter den Sternen statt. Euch erwartet ein Filmspektakel der Extraklasse auf dem Mosbacher Marktplatz – übertragen auf Großleinwand. Kommt vorbei, der Eintritt ist frei!</p>`

Bei fast allen Posts sind auch `img` Tags dabei. In vue können wir diese HTML-Tags *as is* ganz einfach Rendern lassen. Wir referenzieren einfach die Variable innerhalb der vue-eigenen `v-html` *directive* (Anweisung):

```pug
ul.posts
  li.post(v-for="post in getPosts")
    p.right(v-text="formatDateLong(post.date)")
    p(v-html="post.content.rendered")
```
In diesem Template benutzen wir die [Template-Engine Pug](https://github.com/pugjs/pug). Diese ermöglicht uns, schneller HTML zu schreiben, da wir z.B. keine Closing-Tags schreiben müssen. Diese werden automatisch erzeugt.

Leider müssen wir verschiedene Probleme der WordPress-API manuell beheben, damit die korrekte Anzeige der Posts erfolgen kann. Zum Beispiel sind manchmal `<img>`-Tags innerhalb von `<p>`-Tags vorhanden. Durch diesen Aufbau ist es aber nicht möglich, nur die Bilder zu zentrieren.

Dies wird mit einer relativ unübersichtlichen Funktion gelöst, die mit mehreren `indexOf` die Positionen von den "falschen" Tags findet, und dann das HTML so bearbeitet, dass wir es richtig stylen können:

```js
modifyPosts (posts) {
  // Deep copy the array
  let modifiedPosts = JSON.parse(JSON.stringify(posts))
  for (let i in posts) {
    if (posts[i].content.rendered.indexOf('<p><img') !== -1) {
      // `<div class="useless content"><p><img     .../>some text relevant to the image</p>`
      // ^firstPart                       ^secondPart   ^thirdPart (<p> gets moved there)

    // First occurance of the tag
    let index = posts[i].content.rendered.indexOf('<p><img')

    // First Part will usually be empty, if not for some element even before the <p><img> tags
    let firstPart = posts[i].content.rendered.substring(0, index)

    // Second Part starts at the img tag, removing the p tag from `<p><img...`
    let secondPartUnmodified = posts[i].content.rendered.substring(index + 3)

    // Look up at what index the closing bracket for the img tag is at
    let indexImgClose = secondPartUnmodified.indexOf('>')

    // Divide the second part into two pieces and add the p tag inbetween
    let secondPart = secondPartUnmodified.substring(0, indexImgClose + 1)
    let thirdPart = '<p>' + secondPartUnmodified.substring(indexImgClose + 1)
      
    let newContent = firstPart + secondPart + thirdPart
    modifiedPosts[i].content.rendered = newContent
  }
  }
  return modifiedPosts
}
```


# Deployment
Die Web-Anwendung wurde von uns auf einem [Caddy Web-Server](https://github.com/mholt/caddy)  deployed.

Der Server lädt automatisch nach jedem Commit in einem bestimmten Git-Repo die Quelldateien herunter und baut die Anwendung neu.
Sobald dies geschehen ist, wir die neue Anwendung automatisch ausgeführt.

Es muss sich nicht um Caching oder Sonstiges gekümmert werden, da das 
Build-Tool automatisch den Resources Checksums hinzufügt.

Beispiel: `/static/css/app.6c8ac5ef2bd4fd206814eaff5e80a732.css`  
Dank dieser Checksums werden garantiert nur aktuelle Dateien ausgeliefert bzw gecachet. Ohne geeignetes Build-Tool müsste man dies umständlich selbst generieren.

# Screenshots

### Vorlesungen:
![Vorlesungen](https://i.imgur.com/ycGBbKa.png)

### Veranstaltungen:
![Veranstaltungen](https://i.imgur.com/EdW678t.png)

### News:
![News](https://i.imgur.com/ksDouEC.png)
