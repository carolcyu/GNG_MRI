# GNG_MRI
GNG task, designed for NSP study MRI. Based one Michael Souder's updates to GNG_Q repo. Order: third task. 

# Go/No-Go Task (GNG_Q)

A cognitive neuroscience task implementation for measuring response inhibition, designed for MRI/fMRI studies.

## Overview

The Go/No-Go (GNG) task is a classic response inhibition paradigm used in cognitive neuroscience to assess executive function, impulse control, and behavioral regulation. 

## Task Description

### Cognitive Paradigm

The Go/No-Go task measures the ability to suppress prepotent responses. Participants are presented with two types of stimuli:

- **Go Stimulus (Blue Circle)**: Requires a keypress response as quickly as possible
- **No-Go Stimulus (Orange Circle)**: Requires response withholding (no keypress)

### Behavioral Measures

- **Accuracy**: Percentage of correct responses
  - Go trials: Keypress = correct
  - No-Go trials: No keypress = correct
- **Response Time (RT)**: Reaction time for correct Go trials
- **Commission Errors**: False alarms on No-Go trials (pressing when should not)
- **Omission Errors**: Missed responses on Go trials (not pressing when should)

## Installation

### Prerequisites

- Modern web browser (Chrome, Firefox, Safari, Edge)
- Web server (for standalone version) or Qualtrics account (for integrated version)
- Physical keyboard (required for responses)

### Standalone Version

1. Clone or download this repository
2. Serve the files via a web server (local or remote)
3. Open `src/index.html` in a web browser
4. Ensure images are accessible at `src/img/`

## Usage

### MRI Version

1. Open `index.html` in a web browser
2. Follow on-screen instructions
3. Press **1 key** for blue circles (Go trials)
4. Do **not press any key** for orange circles (No-Go trials)
5. Wait for MRI scanner trigger (key **5**) if applicable
6. Complete all trials
7. View accuracy and RT feedback at the end

### Qualtrics Version

1. Participant accesses Qualtrics survey
2. Task loads automatically when reaching the question
3. Press **1 key** for blue circles (Go trials)
4. Do **not press any key** for orange circles (No-Go trials)
5. Wait for MRI scanner trigger (key **5**) if applicable
6. Complete all trials
7. Data automatically saves to Qualtrics embedded data field "GNG"
8. Survey progresses automatically upon completion

## Configuration

Task parameters can be modified in `src/config.json`:

### Stimuli Configuration

- **Go stimulus**: Blue circle image path and response key
- **No-Go stimulus**: Orange circle image path (no response required)

### Timing Parameters

- **Fixation durations**: Variable durations for jittering (500-1000ms)
- **Stimulus durations**: How long stimuli are displayed (standalone: 400ms fixed)
- **Post-trial gap**: Delay between trials (1000ms)

### Trial Configuration

**MRI Version:**
- 16 stimuli in fixed pattern (70% Go, 30% No-Go)
- 5 repetitions
- 80 total trials
- Fixed order (not randomized)

### MRI Configuration

- **Trigger key**: Key '5' (scanner button box)
- **Wait message**: Customizable message during scanner startup

## Data Collection

### Data Format

Each trial records the following information:

```javascript
{
    trial_type: "image-keyboard-response" | "html-keyboard-response",
    stimulus: "path/to/image.png",
    response: "1" | "f" | null,
    rt: number (milliseconds),
    correct: true | false,
    task: "response" | "fixation" | "mri_start",
    correct_response: "1" | "f" | null,
    response: "go" | "no-go"
}
```

### MRI Version

- Data displayed in browser console at experiment end
- Can be copied or exported manually
- Full JSON format available via `jsPsych.data.get().json()`

### Data Analysis

The collected data enables:

- Trial-by-trial accuracy analysis
- Response time distributions
- Error type classification (commission vs. omission)
- Condition-specific analyses (Go vs. No-Go)
- Timing analysis (fixation, stimulus, ITI)
- MRI alignment (with timing data)

## MRI/fMRI Compatibility

### Scanner Integration

- **Trigger synchronization**: Waits for scanner keyboard button '5' before starting test trials
- **Timing optimization**: Variable fixation and stimulus durations to reduce anticipation
- **Visual design**: High contrast, minimal distractions, black background
- **Response collection**: Compatible with MRI-safe keyboards and button boxes

### Timing Considerations

- Variable fixation durations prevent predictable timing patterns
- Variable stimulus durations allow for hemodynamic response modeling
- Fixed trial structure ensures consistent timing across participants
- Post-trial gaps accommodate scanner acquisition timing

## Browser Compatibility

### Supported Browsers

- Chrome (recommended)
- Firefox
- Safari
- Edge

### Requirements

- JavaScript enabled
- Physical keyboard (not touch-only devices)
- Modern browser with ES6 support
- Internet connection (for Qualtrics version with remote hosting)

## Troubleshooting

### Common Issues

**Issue: Images not loading**
- Solution: Ensure image paths are correct relative to HTML file
- Check that images exist in `src/img/` directory
- Verify web server is serving files correctly

**Issue: Keyboard responses not working**
- Solution: Ensure display element has focus (click on screen)
- Check browser console for errors
- Verify keyboard is not blocked by browser extensions

**Issue: Accuracy shows 0% or blank**
- Solution: Check console logs for debugging information
- Verify `correct` field is being set in trial data
- Ensure response trials are being filtered correctly

**Issue: Qualtrics integration not working**
- Solution: Verify GitHub URL is correct and accessible
- Check browser console for script loading errors
- Ensure Qualtrics JavaScript API is available
- Verify display_stage element is created properly

**Issue: Mouse clicks advancing trials**
- Solution: This is expected behavior for instruction screens
- Test trials should not advance on click (response_ends_trial: false)
- If clicks advance test trials, check event handlers

### Debugging

This MRI version includes extensive console logging:

- **MRI**: Look for `[GNG]` prefix in console

### Code Structure

**Standalone Version (`src/index.html`):**
- Direct HTML/JavaScript implementation
- Uses jsPsych framework
- Keyboard-based instruction screens
- 1 key for Go responses
- Local saving of data to desktop as Json file

### Customization

To modify the task:

1. **Change stimuli**: Replace images in `src/img/` or update paths in code
2. **Adjust timing**: Modify duration arrays in `config.json` or code
3. **Change trial count**: Update `repetitions` parameter in test_procedure
4. **Modify instructions**: Edit instruction trial `stimulus` fields
5. **Change response keys**: Update `choices` parameter in test_block


## Version Information

- **Version**: 1.0
- **Framework**: jsPsych 7.x
- **Last Updated**: 2024
- **GitHub Repository**: https://carolcyu.github.io/GNG_MRI/

## Technical Details

### Dependencies

- jsPsych core library
- jsPsych plugin-image-keyboard-response
- jsPsych plugin-html-keyboard-response
- jsPsych plugin-html-button-response
- jsPsych plugin-preload
- jQuery (for Qualtrics version)
- Qualtrics Survey Engine API (for Qualtrics version)

### Performance

- **Load time**: ~1-2 seconds (depending on network)
- **Trial rendering**: Optimized by jsPsych framework
- **Data collection**: Minimal overhead
- **Image loading**: Preloaded to prevent timing issues

### Security and Privacy

- **No external data transmission**: Standalone version runs entirely locally
- **Public repository**: Ensure no sensitive data in code
- **Browser-based**: No server-side data collection

### Cognitive Task

The Go/No-Go task is a well-established paradigm in cognitive neuroscience for measuring response inhibition and executive function. It has been used extensively in fMRI studies to investigate:

- Prefrontal cortex function
- Response inhibition networks
- Impulse control mechanisms
- Executive function deficits

### Framework

- **jsPsych**: https://www.jspsych.org/

## License

This project is provided for research purposes. Please ensure compliance with your institution's research ethics guidelines and data protection regulations.

## Acknowledgments

- jsPsych framework developers
- Qualtrics platform
- Cognitive neuroscience research community

---

**Note**: This task is designed for research purposes in controlled laboratory or clinical settings. Ensure proper IRB approval and participant consent before use in research studies.
