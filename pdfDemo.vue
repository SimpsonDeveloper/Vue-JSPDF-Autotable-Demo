<template>
  <v-container>
    <v-row>
      <v-btn @click="generatePDF1()"> Generate PDF1 </v-btn>
    </v-row>
    <v-row>
      <v-btn @click="generatePDF2()"> Generate PDF2 </v-btn>
    </v-row>
    <v-row>
      <v-btn @click="generatePDF3()"> Generate PDF3 </v-btn>
    </v-row> 
  </v-container>
</template>

<script>
import jsPDF from 'jspdf';
import 'jspdf-autotable';
export default {
  name: "pdfDemo",

  methods: {
    generatePDF1() {
      const doc = new jsPDF({
        orientation: "portrait",
        unit: "mm",
        format: "letter"
      });

      doc.setFontSize(16).text("This is some header", 0, 4);

      doc.autoTable({
          head: [['ID', 'Name', 'Email', 'Country', 'IP-address']],
          body: [
            ['1', 'HelloäöüßÄÖÜ', 'dmoore0@furl.net', 'China', '211.56.242.221'],
            ['2', 'Janice', 'jhenry1@theatlantic.com', 'Ukraine', '38.36.7.199'],
            ['3', 'Ruth', 'rwells2@example.com', 'Trinidad', '19.162.133.184'],
            ['4', 'Jason', 'jray3@psu.edu', 'Brazil', '10.68.11.42'],
            ['5', 'Jane', 'jstephens4@go.com', 'United States', '47.32.129.71'],
            ['6', 'Adam', 'anichols5@com.com', 'Canada', '18.186.38.37'],
          ],
        });

      doc.save("BasicDemo.pdf");
    },

    generatePDF2() {
      const doc = new jsPDF({
        orientation: "portrait",
        unit: "mm",
        format: "letter"
      });

      // Example usage with columnStyles,
      doc.autoTable({
        styles: { fillColor: [255, 0, 0] },
        columnStyles: { 0: { halign: 'center', fillColor: [0, 255, 0] } }, // Cells in first column centered and green
        margin: { top: 10 },
        body: [
          ['Sweden', 'Japan', 'Canada'],
          ['Norway', 'China', 'USA'],
          ['Denmark', 'China', 'Mexico'],
        ],
      });

      // Example usage of columns property. Note that America will not be included even though it exist in the body since there is no column specified for it.
      doc.autoTable({
        columnStyles: { europe: { halign: 'center' } }, // European countries centered
        body: [
          { europe: 'Sweden', america: 'Canada', asia: 'China' },
          { europe: 'Norway', america: 'Mexico', asia: 'Japan' },
        ],
        columns: [
          { header: 'Europe', dataKey: 'europe' },
          { header: 'Asia', dataKey: 'asia' },
        ],
      });

      doc.save("FormatDemo.pdf");
    },

    generatePDF3() {
      var url = "https://picsum.photos/200/300?random=1";
      getImgFromUrl(url, function (img) {
          generatePDF(img);
      });
    }

  },
}

function getImgFromUrl(url, callback) {
  var img = new Image();
  img.src = url;
  img.onload = function () {
      callback(img);
  };
}

function generatePDF(img) {
  var options = {orientation: 'p', unit: 'mm'};
  var doc = new jsPDF(options);
  doc.addImage(img, 'JPEG', 0, 0, 100, 50);
  doc.save("PictureDemo.pdf");
}
</script>