# api-like-mimo
} "likes": 100
}" message": "\u064a\u0631\u062c\0649\u062a\u062r\u064a\064a/u0645\u0625\u064a\u062f\u064a uid={player_id

const WebSocket = require('ws');

// إنشاء خادم WebSocket
const wss = new WebSocket.Server({ port: 8080 });

// تخزين اللاعبين المتصلين
let players = [];

wss.on('connection', (ws) => {
    console.log('A new player connected');
    players.push(ws);

    // استقبال الرسائل من اللاعبين
    ws.on('message', (message) => {
        console.log(`Received: ${message}`);
        // إرسال الرسالة إلى جميع اللاعبين الآخرين
        players.forEach(player => {
            if (player !== ws && player.readyState === WebSocket.OPEN) {
                player.send(message);
            }
        });
    });

    // إزالة اللاعب عند قطع الاتصال
    ws.on('close', () => {
        console.log('A player disconnected');
        players = players.filter(player => player !== ws);
    });
});

console.log('Game server is running on ws://localhost:8080');
