import React, { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Textarea } from '@/components/ui/textarea';

export default function GuruAIGenerator() {
  const [form, setForm] = useState({
    apiKey: '',
    jenis: 'Modul Ajar',
    nama: '',
    mapel: '',
    kelas: '',
    materi: '',
    tujuan: '',
    waktu: '',
  });

  const [hasil, setHasil] = useState('');

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const generateDokumen = async () => {
    const promptHasil = `${form.jenis.toUpperCase()}

Nama Guru: ${form.nama}
Mata Pelajaran: ${form.mapel}
Kelas: ${form.kelas}
Materi: ${form.materi}
Tujuan Pembelajaran: ${form.tujuan}
Alokasi Waktu: ${form.waktu}

Kegiatan Pembelajaran:
1. Pendahuluan
2. Kegiatan Inti
3. Penutup

Catatan: versi preview menggunakan generator lokal.`;

    if (!form.apiKey) {
      setHasil(promptHasil);
      return;
    }

    try {
      setHasil('Sedang membuat dokumen...');

      const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${form.apiKey}`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          contents: [{ parts: [{ text: `Buatkan ${form.jenis} untuk guru dengan data: ${form.nama}, ${form.mapel}, ${form.kelas}, ${form.materi}` }] }]
        })
      });

      const data = await response.json();
      const text = data.candidates?.[0]?.content?.parts?.[0]?.text || promptHasil;
      setHasil(text);
    } catch (error) {
      setHasil(promptHasil);
    }
  };

  const copyHasil = async () => {
    await navigator.clipboard.writeText(hasil);
    alert('Hasil berhasil disalin!');
  };

  const resetForm = () => {
    setForm({
      jenis: 'Modul Ajar',
      nama: '',
      mapel: '',
      kelas: '',
      materi: '',
      tujuan: '',
      waktu: '',
    });
    setHasil('');
  };

  const downloadHasil = () => {
    const blob = new Blob([hasil], { type: 'text/plain' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'dokumen-guru.txt';
    a.click();
    URL.revokeObjectURL(url);
  };

  return (
    <div className="min-h-screen bg-slate-100 p-6 flex justify-center">
      <Card className="w-full max-w-3xl rounded-2xl shadow-lg">
        <CardContent className="p-6 space-y-4">
          <h1 className="text-2xl font-bold">Guru AI Generator</h1>

          <Input placeholder="Gemini API Key" name="apiKey" type="password" onChange={handleChange} />

          <select
            name="jenis"
            onChange={handleChange}
            className="w-full border rounded-md p-2"
            value={form.jenis}
          >
            <option>Modul Ajar</option>
            <option>LKPD</option>
            <option>Soal</option>
            <option>Rubrik Penilaian</option>
          </select>

          <Input placeholder="Nama Guru" name="nama" onChange={handleChange} />
          <Input placeholder="Mata Pelajaran" name="mapel" onChange={handleChange} />
          <Input placeholder="Kelas" name="kelas" onChange={handleChange} />
          <Input placeholder="Materi" name="materi" onChange={handleChange} />
          <Textarea placeholder="Tujuan Pembelajaran" name="tujuan" onChange={handleChange} />
          <Input placeholder="Alokasi Waktu" name="waktu" onChange={handleChange} />

          <Button onClick={generateDokumen} className="w-full">
            Generate Dokumen
          </Button>

          <Button onClick={resetForm} variant="outline" className="w-full">
            Reset Form
          </Button>

          {hasil && (
            <>
              <Button onClick={copyHasil} className="w-full">
                Copy Hasil
              </Button>
              <Button onClick={downloadHasil} variant="outline" className="w-full">
                Download TXT
              </Button>
              <Textarea value={hasil} readOnly className="min-h-[300px]" />
            </>
          )}
        </CardContent>
      </Card>
    </div>
  );
}
