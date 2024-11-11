import React, { useState } from 'react';
import { Calendar, Download, Filter } from 'lucide-react';
import { Card, CardHeader, CardTitle, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Alert, AlertDescription } from '@/components/ui/alert';
import { Select } from '@/components/ui/select';

const HolidayTracker = () => {
  const [holidays, setHolidays] = useState([]);
  const [editingId, setEditingId] = useState(null);
  const [sortBy, setSortBy] = useState('date'); // 'date' or 'duration'
  const [filterCategory, setFilterCategory] = useState('all');
  const [newHoliday, setNewHoliday] = useState({
    title: '',
    startDate: '',
    endDate: '',
    category: 'vacation', // default category
    notes: ''
  });

  const categories = [
    'vacation',
    'personal',
    'sick leave',
    'public holiday',
    'work from home'
  ];

  const addHoliday = () => {
    if (!newHoliday.title || !newHoliday.startDate || !newHoliday.endDate) {
      return;
    }
    
    const start = new Date(newHoliday.startDate);
    const end = new Date(newHoliday.endDate);
    const days = Math.ceil((end - start) / (1000 * 60 * 60 * 24)) + 1;
    
    if (editingId !== null) {
      setHolidays(holidays.map((holiday, index) => 
        index === editingId ? { ...newHoliday, days } : holiday
      ));
      setEditingId(null);
    } else {
      setHolidays([...holidays, { ...newHoliday, days }]);
    }
    
    setNewHoliday({
      title: '',
      startDate: '',
      endDate: '',
      category: 'vacation',
      notes: ''
    });
  };

  const editHoliday = (index) => {
    const holiday = holidays[index];
    setNewHoliday(holiday);
    setEditingId(index);
  };

  const removeHoliday = (index) => {
    const updatedHolidays = holidays.filter((_, i) => i !== index);
    setHolidays(updatedHolidays);
  };

  const sortHolidays = (holidayList) => {
    return [...holidayList].sort((a, b) => {
      if (sortBy === 'date') {
        return new Date(a.startDate) - new Date(b.startDate);
      } else if (sortBy === 'duration') {
        return b.days - a.days;
      }
      return 0;
    });
  };

  const filterHolidays = (holidayList) => {
    if (filterCategory === 'all') return holidayList;
    return holidayList.filter(holiday => holiday.category === filterCategory);
  };

  const exportToCSV = () => {
    const headers = ['Title', 'Start Date', 'End Date', 'Category', 'Days', 'Notes'];
    const rows = holidays.map(h => [
      h.title,
      h.startDate,
      h.endDate,
      h.category,
      h.days,
      h.notes
    ]);
    
    const csvContent = [
      headers.join(','),
      ...rows.map(row => row.join(','))
    ].join('\n');
    
    const blob = new Blob([csvContent], { type: 'text/csv' });
    const url = window.URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'holidays.csv';
    a.click();
    window.URL.revokeObjectURL(url);
  };

  const totalDays = holidays.reduce((sum, holiday) => sum + holiday.days, 0);
  const displayedHolidays = sortHolidays(filterHolidays(holidays));

  const getCategoryColor = (category) => {
    const colors = {
      'vacation': 'bg-blue-100',
      'personal': 'bg-green-100',
      'sick leave': 'bg-red-100',
      'public holiday': 'bg-purple-100',
      'work from home': 'bg-yellow-100'
    };
    return colors[category] || 'bg-gray-100';
  };

  return (
    <div className="max-w-2xl mx-auto p-4">
      <Card className="mb-6">
        <CardHeader>
          <CardTitle className="flex items-center gap-2">
            <Calendar className="h-6 w-6" />
            Holiday Tracker
          </CardTitle>
        </CardHeader>
        <CardContent>
          <div className="space-y-4">
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              <input
                type="text"
                placeholder="Holiday Title"
                className="p-2 border rounded"
                value={newHoliday.title}
                onChange={(e) => setNewHoliday({ ...newHoliday, title: e.target.value })}
              />
              <select
                className="p-2 border rounded"
                value={newHoliday.category}
                onChange={(e) => setNewHoliday({ ...newHoliday, category: e.target.value })}
              >
                {categories.map(cat => (
                  <option key={cat} value={cat}>
                    {cat.charAt(0).toUpperCase() + cat.slice(1)}
                  </option>
                ))}
              </select>
            </div>
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              <input
                type="date"
                className="p-2 border rounded"
                value={newHoliday.startDate}
                onChange={(e) => setNewHoliday({ ...newHoliday, startDate: e.target.value })}
              />
              <input
                type="date"
                className="p-2 border rounded"
                value={newHoliday.endDate}
                onChange={(e) => setNewHoliday({ ...newHoliday, endDate: e.target.value })}
              />
            </div>
            <textarea
              placeholder="Notes (optional)"
              className="w-full p-2 border rounded"
              value={newHoliday.notes}
              onChange={(e) => setNewHoliday({ ...newHoliday, notes: e.target.value })}
            />
            <Button 
              onClick={addHoliday}
              className="w-full"
            >
              {editingId !== null ? 'Update Holiday' : 'Add Holiday'}
            </Button>
          </div>
        </CardContent>
      </Card>

      <div className="flex gap-4 mb-4">
        <select
          className="p-2 border rounded"
          value={sortBy}
          onChange={(e) => setSortBy(e.target.value)}
        >
          <option value="date">Sort by Date</option>
          <option value="duration">Sort by Duration</option>
        </select>
        
        <select
          className="p-2 border rounded"
          value={filterCategory}
          onChange={(e) => setFilterCategory(e.target.value)}
        >
          <option value="all">All Categories</option>
          {categories.map(cat => (
            <option key={cat} value={cat}>
              {cat.charAt(0).toUpperCase() + cat.slice(1)}
            </option>
          ))}
        </select>

        <Button
          onClick={exportToCSV}
          className="flex items-center gap-2"
        >
          <Download className="h-4 w-4" />
          Export
        </Button>
      </div>

      {displayedHolidays.length > 0 ? (
        <div className="space-y-4">
          {displayedHolidays.map((holiday, index) => (
            <Card key={index}>
              <CardContent className="p-4">
                <div className="flex items-center justify-between">
                  <div className="flex-1">
                    <div className="flex items-center gap-2">
                      <h3 className="font-semibold">{holiday.title}</h3>
                      <span className={`px-2 py-1 rounded text-sm ${getCategoryColor(holiday.category)}`}>
                        {holiday.category}
                      </span>
                    </div>
                    <p className="text-sm text-gray-600">
                      {new Date(holiday.startDate).toLocaleDateString()} - {new Date(holiday.endDate).toLocaleDateString()}
                    </p>
                    <p className="text-sm text-gray-600">{holiday.days} days</p>
                    {holiday.notes && (
                      <p className="text-sm text-gray-600 mt-2">{holiday.notes}</p>
                    )}
                  </div>
                  <div className="flex gap-2">
                    <Button 
                      variant="secondary"
                      onClick={() => editHoliday(index)}
                    >
                      Edit
                    </Button>
                    <Button 
                      variant="destructive"
                      onClick={() => removeHoliday(index)}
                    >
                      Remove
                    </Button>
                  </div>
                </div>
              </CardContent>
            </Card>
          ))}
          
          <Alert className="mt-4">
            <AlertDescription>
              Total Holiday Days: {totalDays}
            </AlertDescription>
          </Alert>
        </div>
      ) : (
        <Alert>
          <AlertDescription>
            No holidays added yet. Add your first holiday above!
          </AlertDescription>
        </Alert>
      )}
    </div>
  );
};

export default HolidayTracker;
