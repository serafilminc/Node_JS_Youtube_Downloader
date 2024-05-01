const express = require('express');
const ytdl = require('ytdl-core');

const app = express();
const port = process.env.PORT || 3000; // Use the port provided by Vercel or default to 3000

// Middleware to parse JSON requests
app.use(express.json());

// Middleware for logging requests
app.use((req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
  next();
});

// Define a route for downloading YouTube videos
app.post('/download', async (req, res) => {
  try {
    const { videoUrl } = req.body; // Assuming the client sends the YouTube video URL in the request body

    // Validate the YouTube video URL
    if (!ytdl.validateURL(videoUrl)) {
      return res.status(400).json({ error: 'Invalid YouTube video URL' });
    }

    // Set response headers to stream the video
    res.header('Content-Disposition', 'attachment; filename="video.mp4"');
    ytdl(videoUrl).pipe(res); // Stream the video content directly to the client
  } catch (error) {
    console.error('Error downloading video:', error);
    res.status(500).json({ error: 'Failed to download video' });
  }
});

// Define a route for the root URL (homepage)
app.get('/', (req, res) => {
  res.send('Welcome to the YouTube Downloader!');
});

// Error handling middleware
app.use((err, req, res, next) => {
  console.error('Error:', err.stack);
  res.status(500).json({ error: 'Internal server error' });
});

// Start the server
app.listen(port, () => {
  console.log(`Server is listening at http://localhost:${port}`);
});
