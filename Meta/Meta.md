# BTLO – Meta

**Platform:** Blue Team Labs Online (BTLO)  
**Category:** Digital Forensics  
**Difficulty:** Easy  
**Points:** 10  
**Status:** ✅ Completed  
**Date:** September 2026  

---

## Scenario

The attached images were posted by a criminal on the run, with the caption "I'm roaming free. You will never catch me". We believe you can assist us in proving him wrong.

---

## Theory – Image Metadata (EXIF)

**EXIF (Exchangeable Image File Format)** stores technical and descriptive information inside image files.

Common useful fields for forensics:

| Field | What it tells you |
|-------|-------------------|
| Camera Make / Model | Device used to take the photo |
| Date/Time Original | When the photo was captured |
| Create Date / Modify Date | Creation and last modification times |
| GPS Latitude / Longitude | Location (if present and not altered) |
| Comment / Image Description | Text notes embedded in the file |
| Software | Program that processed the image |

**Important lesson:** Metadata can be altered. Always verify GPS and other fields with alternative methods (reverse image search, visual analysis, etc.).

---

## Tools Used

- **exiftool** – Extract and display metadata from images  
- **7z / p7zip** – Extract password-protected or newer-format ZIP files  
- **Google Images / Google Lens** – Reverse image search for location  

---

## Investigation Steps

### 1. Extract the challenge files

The ZIP uses a newer format that default `unzip` may not support.

```bash
sudo apt install p7zip-full -y
7z x btlo.zip -pbtlo
```

You should get:
- `uploaded_1.JPG`
- `uploaded_2.png`

---

### 2. Extract metadata with exiftool

```bash
sudo apt install libimage-exiftool-perl -y
exiftool uploaded_1.JPG
```

Useful filtered view:

```bash
exiftool uploaded_1.JPG | grep -iE "Camera Model|Date/Time Original|Create Date|Comment|GPS"
```

---

## Answers

### Question 1
**What is the camera model?** (2 points)

**Method:** Look for `Camera Model Name` in exiftool output.

**Answer:** `Canon EOS 550D`

---

### Question 2
**When was the picture taken?** (2 points)

**Method:** Look for `Date/Time Original` or `Create Date`.

**Answer:** `2021:11:02 13:20:23`

---

### Question 3
**What does the comment on the first image say?** (3 points)

**Method:** Look for the `Comment` field.

**Answer:** `relying on altered metadata to catch me?`

---

### Question 4
**Where could the criminal be?** (3 points)

**Method:**
1. GPS coordinates in the metadata are **invalid** (Longitude `279` is outside the valid range of -180 to +180).
2. The comment also indicates metadata was altered.
3. Perform a **reverse image search** (Google Images / Google Lens) on `uploaded_2.png` or `uploaded_1.JPG`.
4. Matching landmarks point to the city.

**Answer:** `Kathmandu`

---

## Summary of Findings

| # | Question | Answer |
|---|----------|--------|
| 1 | Camera model | Canon EOS 550D |
| 2 | Picture taken | 2021:11:02 13:20:23 |
| 3 | Comment | relying on altered metadata to catch me? |
| 4 | Location | Kathmandu |

---

## Key Commands Reference

```bash
# Extract ZIP (newer format / password protected)
7z x btlo.zip -pbtlo

# Full metadata
exiftool uploaded_1.JPG

# Filtered metadata
exiftool uploaded_1.JPG | grep -iE "Camera Model|Date/Time Original|Create Date|Comment|GPS"

# Specific fields only
exiftool -CameraModelName -DateTimeOriginal -CreateDate -Comment uploaded_1.JPG
```

---

## Key Takeaways

- Always start image forensics with **exiftool** (or equivalent)
- GPS and other metadata can be deliberately altered — treat them as untrusted
- Cross-check suspicious location data with **reverse image search**
- Comments and other text fields in EXIF can contain intentional messages or clues
- Validate coordinate ranges (Latitude ±90, Longitude ±180)

---

**Challenge completed and documented.**  
Ready to upload to: https://github.com/vatsayu/BTLO-Writeups
