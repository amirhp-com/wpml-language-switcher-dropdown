# WPML Language Switcher Dropdown with jQuery and Select2

This snippet creates a **custom language switcher dropdown** for WordPress websites using **WPML** (WordPress Multilingual Plugin). It dynamically populates a `<select>` element with available languages and integrates with **Select2** for an enhanced user experience. When a user selects a language, they are redirected to the corresponding translated version of the current page.

---

## Key Features:
1. **Dynamic Language Population**:
   - Fetches available languages using WPML's `icl_get_languages()` function.
   - Displays native language names (e.g., "العربية", "Español") or translated names (e.g., "Arabic", "Spanish") as configured in WPML settings.

2. **Select2 Integration**:
   - Enhances the dropdown with Select2 for better usability and styling.
   - Includes a placeholder and hides the search box for simplicity.

3. **Automatic Redirection**:
   - Redirects users to the selected language's version of the current page.

4. **Customizable**:
   - Easily modify the dropdown to display flags, custom language names, or additional information.

---

## How to Use:
1. Add the provided PHP and jQuery code to your theme's `functions.php` file or a custom plugin.
2. Include the `<select>` element in your theme's `header.php` file where you want the language switcher to appear.
3. Enqueue Select2 scripts and styles if not already included in your theme.
4. Configure language names in WPML settings to customize the displayed text.

---

## Dependencies:
- **WPML Plugin**: Required for multilingual functionality.
- **Select2**: Used for enhanced dropdown styling (optional but recommended).

---

## Example Output:
```
English
العربية
فارسی
Español
```

---

## Disclaimer:
This code is provided **as-is** for educational and informational purposes. While it has been tested and works in standard WordPress environments, it may require adjustments to fit your specific setup. Always test the code on a staging site before deploying it to production.

---

## Warranty-Free Notice:
This snippet is provided **without any warranties or guarantees**. The author is not responsible for any issues, damages, or losses that may arise from using this code. Use it at your own risk.

---

## Developer:
Developed by **[AmirhpCom](https://amirhp.com/)**.  
For more WordPress tips, tricks, and custom solutions, visit my website: [https://amirhp.com/](https://amirhp.com/).

---

## Code

```php
function wpml_language_switcher_script() {
    if (function_exists('icl_get_languages')) {
        ?>
        <script type="text/javascript">
            jQuery(document).ready(function($) {
                // Get available languages for the current page
                const languages = <?php echo json_encode(icl_get_languages('skip_missing=0')); ?>;

                // Get the language switcher dropdown
                const languageSwitcher = $('#wpml-language-switcher');

                if (languageSwitcher.length && languages) {
                    // Clear default options
                    languageSwitcher.empty();

                    // Add available languages to the dropdown
                    $.each(languages, function(langCode, language) {
                        const languageName = language.native_name; // Use native name from WPML
                        const option = $('<option>', {
                            value: language.url, // URL of the translated page
                            text: languageName // Native language name
                        });

                        if (language.active) {
                            option.prop('selected', true); // Mark the current language as selected
                        }

                        languageSwitcher.append(option);
                    });

                    // Initialize Select2
                    languageSwitcher.select2({
                        placeholder: 'Select Language', // Placeholder text
                        minimumResultsForSearch: -1 // Hide the search box
                    });

                    // Handle language selection change
                    languageSwitcher.on('change', function() {
                        const selectedUrl = $(this).val();
                        if (selectedUrl) {
                            window.location.href = selectedUrl; // Redirect to the selected language's page
                        }
                    });
                }
            });
        </script>
        <?php
    }
}
add_action('wp_footer', 'wpml_language_switcher_script');
