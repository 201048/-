# -<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>موقع نشر الصور</title>
  <style>
    body {
      background-color: #000;
      color: #fff;
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
    }

    h1 {
      margin-bottom: 20px;
    }

    #upload-btn, #add-btn {
      background-color: #333;
      color: white;
      padding: 10px 20px;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      cursor: pointer;
      margin: 10px;
    }

    input[type="file"] {
      display: none;
    }

    #preview-section {
      margin-top: 30px;
      border: 2px dashed #555;
      padding: 20px;
      border-radius: 10px;
      max-width: 700px;
      margin-left: auto;
      margin-right: auto;
    }

    .preview-image {
      display: inline-block;
      margin: 10px;
      max-width: 150px;
    }

    .preview-image img {
      max-width: 100%;
      border-radius: 10px;
    }

    #gallery {
      margin-top: 40px;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 20px;
    }

    .image-container {
      position: relative;
      width: 200px;
    }

    .image-container img {
      width: 100%;
      border-radius: 10px;
    }

    .download-btn {
      position: absolute;
      bottom: 10px;
      left: 50%;
      transform: translateX(-50%);
      background-color: rgba(0, 0, 0, 0.7);
      color: white;
      border: none;
      padding: 5px 10px;
      border-radius: 5px;
      cursor: pointer;
      text-decoration: none;
    }
  </style>
</head>
<body>
  <h1>موقع نشر الصور</h1>

  <!-- زر رفع الصور -->
  <label for="image-input" id="upload-btn">ضع</label>
  <input type="file" id="image-input" accept="image/*" multiple>

  <!-- معاينة الصور -->
  <div id="preview-section" style="display: none;">
    <h3>الصور التي تم تحميلها:</h3>
    <div id="preview-images"></div>
    <button id="add-btn">أضف إلى الموضوع</button>
  </div>

  <!-- معرض الصور -->
  <div id="gallery"></div>

  <script>
    const imageInput = document.getElementById('image-input');
    const previewSection = document.getElementById('preview-section');
    const previewImages = document.getElementById('preview-images');
    const addBtn = document.getElementById('add-btn');
    const gallery = document.getElementById('gallery');

    let imagesToAdd = [];

    imageInput.addEventListener('change', () => {
      const files = Array.from(imageInput.files);
      imagesToAdd = [];

      previewImages.innerHTML = ''; // مسح المعاينات السابقة

      files.forEach(file => {
        if (file.type.startsWith('image/')) {
          const reader = new FileReader();
          reader.onload = function(e) {
            imagesToAdd.push({ src: e.target.result, name: file.name });

            const div = document.createElement('div');
            div.classList.add('preview-image');
            div.innerHTML = `<img src="${e.target.result}" alt="صورة">`;
            previewImages.appendChild(div);
          };
          reader.readAsDataURL(file);
        }
      });

      if (files.length > 0) {
        previewSection.style.display = 'block';
      }
    });

    addBtn.addEventListener('click', () => {
      imagesToAdd.forEach(image => {
        const container = document.createElement('div');
        container.classList.add('image-container');

        const img = document.createElement('img');
        img.src = image.src;

        const downloadLink = document.createElement('a');
        downloadLink.href = image.src;
        downloadLink.download = image.name;
        downloadLink.className = 'download-btn';
        downloadLink.textContent = 'تنزيل';

        container.appendChild(img);
        container.appendChild(downloadLink);
        gallery.appendChild(container);
      });

      // إعادة تعيين
      previewSection.style.display = 'none';
      previewImages.innerHTML = '';
      imageInput.value = '';
      imagesToAdd = [];
    });
  </script>
</body>
</html>
