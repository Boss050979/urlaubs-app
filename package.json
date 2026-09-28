const express = require('express');
const fs = require('fs');
const path = require('path');

const app = express();
const PORT = process.env.PORT || 3000;
const DATA_FILE = path.join(__dirname, 'database.json');

app.use(express.json());
app.use(express.static(path.join(__dirname, 'public')));

// Standard-Daten falls noch keine Datenbank existiert
const initialData = {
    employees: [
        { id: '1', name: 'Mustafa Yilmaz', dept: 'Fahrdienst', vacationDays: 30, extraDaysByYear: {} },
        { id: '2', name: 'Anna Schmidt', dept: 'Disposition', vacationDays: 30, extraDaysByYear: {} },
        { id: '3', name: 'Thomas Weber', dept: 'Fahrdienst', vacationDays: 28, extraDaysByYear: {} }
    ],
    entries: []
};

// Hilfsfunktion: Daten laden
function loadData() {
    if (!fs.existsSync(DATA_FILE)) {
        fs.writeFileSync(DATA_FILE, JSON.stringify(initialData, null, 2));
    }
    try {
        const raw = fs.readFileSync(DATA_FILE, 'utf8');
        return JSON.parse(raw);
    } catch (err) {
        return initialData;
    }
}

// Hilfsfunktion: Daten speichern
function saveData(data) {
    fs.writeFileSync(DATA_FILE, JSON.stringify(data, null, 2));
}

// REST-Endpunkte
app.get('/api/data', (req, res) => {
    res.json(loadData());
});

app.post('/api/data', (req, res) => {
    const { employees, entries } = req.body;
    if (employees && entries) {
        saveData({ employees, entries });
        res.json({ success: true });
    } else {
        res.status(400).json({ error: 'Ungültige Datenstruktur' });
    }
});

// Server starten
app.listen(PORT, () => {
    console.log(`🚀 Urlaubsplaner läuft unter: http://localhost:${PORT}`);
});