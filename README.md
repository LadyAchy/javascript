const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const PORT = process.env.PORT || 5000;

app.use(cors());
app.use(bodyParser.json());

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/jobPlatform', {
    useNewUrlParser: true,
    useUnifiedTopology: true
});

// Define Job Schema
const jobSchema = new mongoose.Schema({
    title: String,
    description: String,
    budget: Number,
    skills: [String],
    postedBy: String
});

const Job = mongoose.model('Job', jobSchema);

// Routes
app.get('/jobs', async (req, res) => {
    const jobs = await Job.find();
    res.json(jobs);
});

app.post('/jobs', async (req, res) => {
    const newJob = new Job(req.body);
    await newJob.save();
    res.json(newJob);
});

app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
