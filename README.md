const express = require('express');
const http = require('http');
const socketIo = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIo(server);

io.on('connection', (socket) => {
    socket.on('signal', (data) => {
        io.to(data.to).emit('signal', data);
    });

    socket.on('join', (roomId) => {
        socket.join(roomId);
        io.to(roomId).emit('ready', { socketId: socket.id });
    });
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => console.log(`Server is running on port ${PORT}`));
