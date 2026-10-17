# Callbacks

OCR success is `OCRResult` JSON passed directly to `veryfiLensSuccess`. It does not use the package envelope `status: "done"`.

| Key | Meaning |
|-----|---------|
| `ocr_text` | Matched text. `""` if nil |
| `ocr_score` | Double |
| `ocr_uuid` | Local id. Also the document-ready `session_id` |
| `ocr_image` | `Documents/ocrImage.jpg` after success saves the crop. `""` on the live update |
| `ocr_type` | `"caps"` on the processed callback, because the presenter sets `ocrType` from settings. The live update path does not set `ocrType`, so the key is `""` there |

`veryfiLensUpdate` is the live payload, same keys, image still empty.

`veryfiLensClose` follows the scheduled camera dismiss (`imageProcessorDidProcessOCR` sends success, then `closeLens()` on the main queue).

`veryfiLensError` is configure and credential failure. OCR mismatch strings are **Unconfirmed**. A read that never matches simply does not call success.

Document-ready uses `document_type` `mobile_ocr` and includes `ocr_text` and `ocr_score`. The delegate payload is the table above, not that request body. This is not a credit-card PAN path.

`PackageUploader.inAppAPIProcessing` is the credit-card package path. This camera path does not wait for it.

`VeryfiLens.shared().shareOCRFeedback()` shares the last code-scan logs. It does not return the OCR text. Call it only when the host wants to send those logs.

Whether auto-delete removes `Documents/ocrImage.jpg` is **Unconfirmed**. Copy the path during success if the host needs the file.
