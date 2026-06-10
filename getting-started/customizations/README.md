# Customizations

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        :root {
            --primary-color: #4f46e5; /* Modern Indigo */
            --primary-hover: #4338ca;
            --text-main: #1f2937;
            --text-muted: #4b5563;
            --bg-input: #f9fafb;
            --border-color: #d1d5db;
        }

        .consultation-form-card {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background: #ffffff;
            padding: 40px;
            border-radius: 16px;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05), 0 8px 10px -6px rgba(0, 0, 0, 0.05);
            width: 100%;
            max-width: 480px;
            box-sizing: border-box;
            margin: 20px auto;
            border: 1px solid #e5e7eb;
        }

        .form-header {
            text-align: center;
            margin-bottom: 28px;
        }

        .form-header h2 {
            margin: 0 0 8px 0;
            color: var(--text-main);
            font-size: 24px;
            font-weight: 700;
            letter-spacing: -0.5px;
        }

        .form-header p {
            margin: 0;
            color: var(--text-muted);
            font-size: 14px;
            line-height: 1.5;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            margin-bottom: 6px;
            color: var(--text-main);
            font-size: 14px;
            font-weight: 500;
        }

        .form-group input,
        .form-group select,
        .form-group textarea {
            width: 100%;
            padding: 12px 14px;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            background-color: var(--bg-input);
            font-size: 14px;
            color: var(--text-main);
            font-family: inherit;
            box-sizing: border-box;
            transition: all 0.2s ease;
        }

        .form-group input:focus,
        .form-group select:focus,
        .form-group textarea:focus {
            outline: none;
            border-color: var(--primary-color);
            background-color: #ffffff;
            box-shadow: 0 0 0 4px rgba(79, 70, 229, 0.12);
        }

        .form-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 16px;
        }

        @media (max-width: 480px) {
            .form-row {
                grid-template-columns: 1fr;
                gap: 0;
            }
        }

        .submit-btn {
            width: 100%;
            padding: 14px;
            background-color: var(--primary-color);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.2s ease, transform 0.1s ease;
            margin-top: 10px;
        }

        .submit-btn:hover {
            background-color: var(--primary-hover);
        }

        .submit-btn:active {
            transform: scale(0.99);
        }
    </style>
</head>
<body>

<div class="consultation-form-card">
    <div class="form-header">
        <h2>Claim Your Free Consultation</h2>
        <p>Tell us a bit about your goals, and we'll schedule a free discovery call to see how we can help.</p>
    </div>

    <form action="https://api.web3forms.com/submit" method="POST">
        
        <input type="hidden" name="access_key" value="YOUR_ACCESS_KEY_HERE">

        <div class="form-group">
            <label for="name">Full Name</label>
            <input type="text" id="name" name="name" placeholder="John Doe" required>
        </div>

        <div class="form-group">
            <label for="email">Email Address</label>
            <input type="email" id="email" name="email" placeholder="john@example.com" required>
        </div>

        <div class="form-row">
            <div class="form-group">
                <label for="phone">Phone Number</label>
                <input type="tel" id="phone" name="phone" placeholder="(555) 000-0000">
            </div>
            <div class="form-group">
                <label for="time">Preferred Time</label>
                <select id="time" name="preferred_time">
                    <option value="morning">Morning (9 AM - 12 PM)</option>
                    <option value="afternoon">Afternoon (12 PM - 5 PM)</option>
                    <option value="evening">Evening (5 PM - 8 PM)</option>
                </select>
            </div>
        </div>

        <div class="form-group">
            <label for="message">What are you looking to achieve?</label>
            <textarea id="message" name="message" rows="4" placeholder="Briefly describe your project or what you need help with..." required></textarea>
        </div>

        <input type="checkbox" name="botcheck" class="hidden" style="display: none;">

        <button type="submit" class="submit-btn">Book Free Consultation</button>
    </form>
</div>

</body>
</html>
